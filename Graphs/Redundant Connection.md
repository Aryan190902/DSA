**Question**: [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

**Approach**:
- Find the largest node label to size the disjoint set union (DSU) arrays. Each node starts in its own component.
- Process edges in input order. The DSU tracks the connected components formed by the edges processed so far. If an edge joins two different components, merge them using union by rank. If its endpoints already share a representative, there is already a path between them and the edge closes a cycle.
- A tree with one extra edge has exactly one cycle. When the last edge of that cycle appears in the input, all the other cycle edges already form a path between its endpoints. This is the edge the DSU rejects, and removing it leaves a tree.

TC: $O(n\alpha(n))$ amortized, where $n$ is both the number of nodes and the number of edges, and $\alpha$ is the inverse Ackermann function. Finding the largest label takes $O(n)$, followed by one union attempt per edge.

SC: $O(n)$ for the DSU arrays, including up to $O(\log n)$ recursive find frames under union by rank.

**Code**:
```cpp
class DSU{
    vector<int>rank, size, parent;
public:
    DSU(int n){
        parent.resize(n+1);
        rank.resize(n+1, 0);
        size.resize(n+1, 1);

        // Each node starts as the representative of its own component.
        for(int i = 0 ; i <= n ; i++)   parent[i] = i;
    }

    int findUltimateParent(int node){
        if(parent[node] == node)    return node;

        // Path compression points this node directly to the root on the way back.
        return parent[node] = findUltimateParent(parent[node]);
    }

    // Return whether the edge merges two distinct components.
    bool unionByRank(int u, int v){
        int upU = findUltimateParent(u);
        int upV = findUltimateParent(v);

        // A shared root means an earlier path already connects the endpoints.
        if(upU == upV)  return false;

        // Attach the lower-rank root under the higher-rank root.
        if(rank[upU] < rank[upV]){
            parent[upU] = upV;
        }
        else if(rank[upU] > rank[upV]){
            parent[upV] = upU;
        }
        else{
            // When ranks tie, the surviving root's height bound increases by one.
            parent[upV] = upU;
            rank[upU]++;
        }

        return true;
    }

    // This unused helper compares ranks and adds sizes at the surviving root.
    void unionBySize(int u, int v){
        int upU = findUltimateParent(u);
        int upV = findUltimateParent(v);

        if(rank[upU] < rank[upV]){
            parent[upU] = upV;
            size[upV] += size[upU];
        }
        else{
            parent[upV] = upU;
            size[upU] += size[upV];
        }
    }

};


class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        // Labels start at 1, so allocate through the largest label seen.
        int n = 0;
        for(int i = 0 ; i < edges.size() ; i++){
            int x = edges[i][0], y = edges[i][1];
            n = max(n, max(x, y));
        }
        DSU ds(n);
        vector<int>ans;
        for(int i = 0 ; i < edges.size() ; i++){
            if(!ds.unionByRank(edges[i][0], edges[i][1])){
                // The other edges of the unique cycle have already appeared.
                ans = {edges[i][0], edges[i][1]};
            }
        }
        return ans;
    }
};
```
