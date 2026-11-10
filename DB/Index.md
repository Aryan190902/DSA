# Database Indexes: Visual Notes

## 1. The problem indexes solve

A database stores rows in **pages** on disk. Without an index, a query reads page after page and tests every row (a **full table scan**, O(N)). With millions of rows this is very slow.

An **index** is a separate lookup structure that records where a value lives, so the database can jump straight to the right page.

```mermaid
flowchart LR
    Q["Query: WHERE customer_id = 4242"] --> D{"Index on customer_id?"}
    D -- No --> S["Seq Scan: read every page"]
    S --> S2["Test all 100,000 rows, discard 99,991"]
    S2 --> R1["9 rows in about 10.4 ms"]
    D -- Yes --> I["Index lookup: find matching row locations"]
    I --> P["Read only the needed pages"]
    P --> R2["9 rows in about 0.2 ms"]
```

My EXPLAIN ANALYZE run: Seq Scan (cost 2037) became Bitmap Index Scan + Bitmap Heap Scan (cost about 41), roughly 50x faster.

## 2. Types of indexes

```mermaid
flowchart TD
    A["Database indexes"] --> B["B-Tree"]
    A --> C["Hash index"]
    A --> D["Geospatial index"]
    A --> E["Inverted index"]
    B --> B1["Default. Sorted, supports ranges"]
    C --> C1["Exact match only"]
    D --> D1["2D data: Geohash, Quadtree, R-Tree"]
    E --> E1["Text search: word maps to documents"]
```

### 2.1 B-Tree

A self-balancing tree. Each node holds a sorted list of keys and pointers to child nodes; leaves point to the rows or pages. Lookups are O(log N), and because keys are sorted it supports equality, ranges and ORDER BY.

**Point lookup: search for 45**

```mermaid
flowchart TD
    R["Root: 30 | 60"] -->|"45 is between 30 and 60"| N2["Node: 40 | 50"]
    R -.-> N1["Node: 10 | 20"]
    R -.-> N3["Node: 70 | 80"]
    N2 -->|"45 is between 40 and 50"| L3["Leaf: 42, 45"]
    N2 -.-> L4["Leaf: 55, 58"]
    L3 --> F["Found 45: follow pointer to the page"]
```

**Range query: WHERE value BETWEEN 42 AND 58** (leaves are linked, so after finding the start the database just walks right)

```mermaid
flowchart LR
    S["Find 42 via the tree"] --> L3["Leaf: 42, 45"]
    L3 -->|next leaf| L4["Leaf: 50, 55"]
    L4 -->|next leaf| L5["Leaf: 58, 61"]
    L5 --> E["Stop after passing 58"]
```

### 2.2 Hash index

Works like a hash map (closer to `unordered_map`). Hash the key, jump to the bucket. Fast for equality, useless for ranges because there is no ordering.

```mermaid
flowchart LR
    K["Key: user_42"] --> H["Hash function"]
    H --> B3["Bucket 3"]
    B0["Bucket 0"]
    B1["Bucket 1"]
    B2["Bucket 2"]
    B3 --> RW["Row pointer: page 17"]
    Q2["WHERE id greater than 40"] --> X["Not possible: no ordering, so full scan"]
```

Redis is a key-value store built on hash tables, which is related but not the same as a hash index inside an RDBMS. PostgreSQL has hash indexes but they are rarely used.

### 2.3 Geospatial indexes

B-trees are great for 1D data but poor for 2D data (lat/long), because one sorted order cannot keep nearby points close in both dimensions.

#### Geohash

Encodes a location into a string. Each extra character narrows the area, and nearby places share a prefix, so a nearby search becomes a prefix search that a B-tree can handle.

```mermaid
flowchart TD
    A["9: large region"] --> B["9q: smaller region"]
    B --> C["9q8: city-sized area"]
    C --> D["9q8y: neighbourhood"]
    D --> E["9q8yy: a few blocks"]
    C --> F["9q8z: neighbouring area, same 9q8 prefix"]
```

#### Quadtree

Recursively splits space into 4 quadrants. A quadrant is split again only when it holds too many points, so dense areas get finer cells.

```mermaid
flowchart TD
    W["Whole map"] --> NW["NW: few points, stop"]
    W --> NE["NE: crowded, split again"]
    W --> SW["SW: few points, stop"]
    W --> SE["SE: few points, stop"]
    NE --> NE1["NE-NW"]
    NE --> NE2["NE-NE"]
    NE --> NE3["NE-SW"]
    NE --> NE4["NE-SE: contains the target point"]
```

Searching for a point means walking down: map, NE, NE-SE. Only that branch is read.

#### R-Tree

Groups nearby objects into nested bounding rectangles. A search skips every rectangle that does not overlap the search area.

```mermaid
flowchart TD
    Root["Root"] --> R1["Rectangle R1: north part of city"]
    Root --> R2["Rectangle R2: south part of city"]
    R1 --> C1["Cafe A"]
    R1 --> C2["Cafe B"]
    R2 --> C3["Cafe C"]
    R2 --> C4["Cafe D"]
    SB["Search box overlaps R1 only"] -.-> R1
    SB -.->|"no overlap, skip"| R2
```

### 2.4 Inverted index

Built for text search. Instead of document to words, it stores word to documents.

```mermaid
flowchart LR
    subgraph Docs
        D1["Doc 1: cats love milk"]
        D2["Doc 2: dogs love bones"]
        D3["Doc 3: cats chase dogs"]
    end
    subgraph Index
        W1["cats: Doc 1, Doc 3"]
        W2["dogs: Doc 2, Doc 3"]
        W3["love: Doc 1, Doc 2"]
    end
    D1 --> W1
    D3 --> W1
    D2 --> W2
    D3 --> W2
    D1 --> W3
    D2 --> W3
```

**Query: cats AND dogs**

```mermaid
flowchart LR
    A["cats: Doc 1, Doc 3"] --> I{"Intersect"}
    B["dogs: Doc 2, Doc 3"] --> I
    I --> R["Result: Doc 3"]
```

## 3. Which index should I use?

```mermaid
flowchart TD
    Start["What am I searching?"] --> T{"Data type"}
    T -- "Text, keywords" --> INV["Inverted index"]
    T -- "Lat/long, maps" --> GEO["Geohash / Quadtree / R-Tree"]
    T -- "Numbers, strings, dates" --> Q{"Need range or ORDER BY?"}
    Q -- Yes --> BT["B-Tree"]
    Q -- "No, equality only" --> BT2["B-Tree (safe default) or Hash"]
```

Rule of thumb: when in doubt, use a B-tree.

## 4. The trade-off

| | Without index | With index |
|---|---|---|
| Reads | Slow (scan all) | Fast (O(log N) for B-tree) |
| Writes (INSERT / UPDATE / DELETE) | Fast | Slower, every index must be updated |
| Disk space | Table only | Table + each index |

```mermaid
flowchart LR
    W["INSERT a new row"] --> T["Write to table"]
    W --> I1["Update index 1"]
    W --> I2["Update index 2"]
    W --> I3["Update index 3"]
```

Indexes pay off for selective filters. If a filter matches a large share of the table, the planner may skip the index and do a Seq Scan anyway.

## 5. Summary

| Index | Best for | Weakness |
|---|---|---|
| B-Tree | Equality, ranges, sorting | Poor for 2D data |
| Hash | Exact match | No ranges or ordering |
| Geohash | Nearby search on a B-tree | Cells have edge cases at boundaries |
| Quadtree | Uneven point density | Deep trees in crowded areas |
| R-Tree | Areas and rectangles | More complex to maintain |
| Inverted | Full-text search | Larger storage, costly updates |