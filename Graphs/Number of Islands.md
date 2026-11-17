**Question**: [Number of Islands](https://leetcode.com/problems/number-of-islands/)

**Approach**:
- Traverse the grid cell by cell.
- When you find an unvisited land cell (`'1'`), start a DFS/BFS from it and mark every connected land cell as visited.
- Each DFS explores all four directions: up, down, left, and right.
- Count one island for each connected component of land cells.

TC: `O(n * m)`, since each cell is visited at most once.
SC: `O(n * m)` for the visited array, plus recursion stack in the worst case for a fully connected landmass.

**Code**:
```cpp
class Solution {
public:
    void solve(vector<vector<char>>& g, vector<vector<int>>& vis, int i, int j) {
        // Stop if we move outside the grid.
        if (i < 0 || j < 0 || i >= (int)g.size() || j >= (int)g[0].size()) return;
        // Skip visited cells and water cells.
        if (vis[i][j] || g[i][j] == '0') return;

        vis[i][j] = 1;

        // Explore all four directions from this land cell.
        solve(g, vis, i + 1, j); // down
        solve(g, vis, i, j + 1); // right
        solve(g, vis, i - 1, j); // up
        solve(g, vis, i, j - 1); // left
    }

    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> vis(n, vector<int>(m, 0));
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == '1' && !vis[i][j]) {
                    solve(grid, vis, i, j); // Mark one complete island as visited.
                    ans++;
                }
            }
        }

        return ans;
    }
};
```