**Question**: [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

A province is a connected component of the undirected graph represented by the adjacency matrix. Let $n$ be the number of cities.

### Depth-first search

**Approach**:
- Start a DFS from each unvisited city. Mark each city on entry and scan its row to find unvisited neighbors.
- Each DFS visits every city reachable from its starting city. Count one province per DFS started by the outer loop, including a city with no neighbors.

TC: $O(n^2)$ because DFS scans one row of length $n$ for each city.

SC: $O(n)$ for the visited array and the recursion stack.

**Code**:
```cpp
class Solution {
public:

    // Visit every city reachable from idx through direct or indirect connections.
    void solve(vector<vector<int>>&v, vector<int>&vis, int idx){
        // Mark on entry so a neighbor cannot recurse back into this city.
        vis[idx] = 1;
        for(int i = 0 ; i < (int)v.size() ; i++){
            // Row idx lists direct connections. The diagonal entry represents idx itself.
            if(v[idx][i] == 1 && !vis[i] && i != idx){
                solve(v, vis, i);
            }
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int ans = 0;
        int n = isConnected.size();
        vector<int>vis(n, 0);
        // An unvisited city belongs to a province no earlier DFS has reached.
        for(int i = 0 ; i < n ; i++){
            if(!vis[i]){
                solve(isConnected, vis, i);
                ans++; // The DFS has marked the entire province, so count it once.
            }
        }
        return ans;
    }
};
```

### Disjoint set union

**Approach**:
- Start with $n$ separate components. Scan the entries above the diagonal so each pair of distinct cities is checked once.
- For each connection, find the two cities' representatives using path compression. Union by rank merges the components only when their representatives differ.
- Decrement the province count for each successful merge. Multiple connections within the same component leave the count unchanged.

TC: $O(n^2 + E\alpha(n))$ amortized, where $E$ is the number of directly connected pairs and $\alpha$ is the inverse Ackermann function. The matrix scan costs $O(n^2)$, and each union attempt costs $O(\alpha(n))$ amortized.

SC: $O(n)$ for the DSU arrays, including up to $O(\log n)$ recursive find frames under union by rank.

**Code**:
```cpp
class DSU{
    // Each set is a tree. Its root represents the component.
    vector<int>rank, parent, size;
    public:
    DSU(int n){
        rank.resize(n+1, 0);
        size.resize(n+1, 1);
        parent.resize(n+1);

        // Initially every node is the root of its own singleton set.
        for(int i = 0 ; i <= n ; i++){
            parent[i] = i;
        }
    }

    int findUltimateParent(int node){
        if(parent[node] == node){
            return node;
        }
        // Path compression points this node directly to the root on the way back.
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Report whether this connection merges two distinct components.
    bool unionByRank(int u, int v){
        int upU = findUltimateParent(u);
        int upV = findUltimateParent(v);
        if(upU == upV)  return false; // A shared root means the cities are already connected.

        // Attach the lower-rank tree under the higher-rank root.
        if(rank[upU] > rank[upV]){
            parent[upV] = upU;
        }
        else if(rank[upU] < rank[upV]){
            parent[upU] = upV;
        }else{
            // Equal ranks can increase the height bound by one at the new root.
            parent[upU] = upV;
            rank[upV]++;
        }
        return true;
    }

    // Alternative merge for distinct sets with current sizes. This solution uses rank.
    void unionBySize(int u, int v){
        int upU = findUltimateParent(u);
        int upV = findUltimateParent(v);

        // Attach the smaller set and store the combined size at the surviving root.
        if(size[upU] > size[upV]){
            parent[upV] = upU;
            size[upU] += size[upV];
        }
        else{
            parent[upU] = upV;
            size[upV] += size[upU];
        }
    }
};

class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        DSU ds(n);
        int ans = n; // Each city starts in its own province.
        // Scan above the diagonal to consider each undirected connection once.
        for(int i = 0 ; i < n ; i++){
            for(int j = i+1 ; j < n ; j++){
                // Count one fewer province only when an edge joins separate components.
                if(isConnected[i][j] == 1 && ds.unionByRank(i, j))  ans--;
            }
        }

        return ans;
    }
};
```
