# Cache-Aside with Thundering Herd Prevention

## Problem

A hot key expires and many concurrent requests miss the cache at once. All of them query the database with the same expensive query, and the DB gets overloaded (cache stampede).

## Design summary

| Technique | Purpose |
|---|---|
| Cache-aside | App reads the cache first and falls back to the DB on a miss |
| TTL jitter | Randomise expiry (e.g. 300s ± 10%) so keys don't all expire together |
| Distributed lock (`SET NX PX`) | Only one request recomputes the value |
| Lock TTL | If the lock holder crashes, the lock expires and another request takes over |
| Stale fallback | Requests that lose the lock get the old value instead of blocking or hitting the DB |

## Sequence diagram

> To render this in VS Code, install the **Markdown Preview Mermaid Support** extension, then open the preview (`Ctrl+Shift+V`).

```mermaid
sequenceDiagram
    autonumber
    actor Client
    participant App as App Server
    participant Redis as Redis (cache + lock)
    participant DB as Database

    Client->>App: GET /dashboard/summary
    App->>Redis: GET cache:summary
    Redis-->>App: value + logical expiry (or nil)

    alt Cache hit and still fresh
        App-->>Client: 200 cached data
    else Cache miss or logically expired
        App->>Redis: SET lock:summary token NX PX 5000

        alt Lock acquired (this request is the leader)
            Redis-->>App: OK
            App->>DB: Run expensive query
            DB-->>App: Result
            App->>Redis: SET cache:summary (TTL = base + random jitter)
            App->>Redis: Release lock (delete only if token matches)
            App-->>Client: 200 fresh data
        else Lock not acquired (another request is refreshing)
            Redis-->>App: nil
            alt Stale value exists
                App-->>Client: 200 stale data (no DB hit)
            else Cold start, no value at all
                App->>Redis: Short bounded wait, then re-check cache
                alt Cache populated meanwhile
                    Redis-->>App: fresh value
                    App-->>Client: 200 fresh data
                else Still empty after timeout
                    App-->>Client: 503 Retry-After
                end
            end
        end
    end

    Note over Redis: If the lock holder crashes mid-recompute,<br/>the lock key expires after its TTL (5s)<br/>and the next request can take over.
```

## Notes

- **Stale fallback implementation:** store the value with a *logical* expiry (soft TTL) inside the payload, and give the Redis key a longer *physical* TTL. After the logical expiry the value is "stale but still servable" while one request refreshes it.
- **Lock TTL sizing:** slightly longer than the normal recompute time. Too short and the lock expires mid-recompute, causing duplicate DB queries. Too long and a crashed holder blocks refreshes for longer.
- **Safe release:** store a unique token as the lock value and release it with a Lua script (`if GET == token then DEL`), so a slow request can't delete a lock that now belongs to someone else.
- **Multiple servers:** the lock lives in Redis, so it works across all app servers. An in-process mutex (singleflight) would only deduplicate per server.
- **Invalidation on writes:** when the underlying data changes, delete or overwrite `cache:summary` explicitly. TTL handles the rest.
- **Alternative: background refresh via queue.** Requests only read the cache, and a worker rebuilds the value. It needs the same two guards: deduplicate refresh jobs per key (an atomic `SET NX` again) and a visibility timeout so a crashed worker's job is re-delivered.