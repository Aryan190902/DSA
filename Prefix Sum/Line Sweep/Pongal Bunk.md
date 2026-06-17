**Question**: [Pongal Bunk](https://www.codechef.com/problems/COWA19B?tab=statement)

**Approach**: 
- Since for each index, the value added is `i-L+1`, we can rewrite it as: `i+1-L`.
- `i+1` will be constant for each query, so if we can just have a track of # queries performed on this index, and sum of all the *L* from the queries, we can calculate the answer.
- After tracking both # queries, and sum of different *l*, the answer would be  `(i+1)* (# of queries) - (sum of different l)`
- NOTE: We are given position instead of indices, that's why we substract 1.

TC: `O(q+n+m)`
SC: `O(n)`

**Code**:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, q, m;
    
    // Array length
    cin>>n;
    vector<int>sm(n, 0), pre(n, 0);
    /*
    sm = tracks how many queries are active at index 
    pre = tracks the accumulated L values of active queries
    */
    
    // Queries length
    cin>>q;
    for(int i = 0 ; i < q ; i++){
        int l, r;
        cin>>l>>r;
        
        // Active query
        sm[l-1] += 1;
        if(r < n)   sm[r] -= 1;
        
        // Accumulate 
        pre[l-1] += l-1;
        if(r < n)   pre[r] -= l-1;
    }
    
    for(int i = 1 ; i < n ; i++){
        sm[i] += sm[i-1];
        pre[i] += pre[i-1];
    }

    // M indices
    cin>>m;
    for(int i = 0 ; i < m ; i++){
        int idx;
        cin>>idx;
        int ans = idx*sm[idx-1] - pre[idx-1];
        cout << ans << endl;
    }
    
    return 0;
}

```