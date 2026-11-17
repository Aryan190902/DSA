**Question**: [Rotten Oranges](https://leetcode.com/problems/rotting-oranges/)

**Approach**:
- Start a BFS from every initially rotten orange, recording the time step at which each orange becomes rotten.
- Process neighbors in four directions; whenever a fresh orange is infected, enqueue it with the current time plus one.
- Track how many fresh oranges remain. If any remain after BFS finishes, the grid can never become fully rotten and the answer is `-1`.
- The maximum time recorded across all rotten oranges is the minimum number of minutes needed to rot all fresh oranges.

TC: `O(n * m)`, because each cell is processed at most once.
SC: `O(n * m)` for the queue and visited array in the worst case.

**Code**:
```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        queue<pair<pair<int, int>, int>> q; // {cell position, minutes elapsed}
        vector<vector<int>> vis(grid.size(), vector<int>(grid[0].size(), 0));
        int fresh = 0;

        // Initialize BFS with all rotten oranges and count fresh oranges.
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == 2) {
                    q.push({{i, j}, 0});
                    vis[i][j] = 1;
                } else if (grid[i][j] == 1) {
                    fresh++;
                }
            }
        }

        int ans = 0;
        vector<int> x{0, 0, 1, -1};
        vector<int> y{1, -1, 0, 0};

        while (!q.empty()) {
            pair<pair<int, int>, int> p = q.front();
            q.pop();

            int i = p.first.first, j = p.first.second, t = p.second;

            // Spread rot to neighboring fresh oranges.
            for (int a = 0; a < 4; a++) {
                int dx = i + x[a], dy = j + y[a];
                if (dx >= 0 && dy >= 0 && dx < grid.size() && dy < grid[0].size() &&
                    !vis[dx][dy] && grid[dx][dy] == 1) {
                    q.push({{dx, dy}, t + 1});
                    vis[dx][dy] = 1;
                    fresh--;
                }
            }

            ans = max(ans, t);
        }

        if (fresh > 0) return -1; // Some oranges could never be reached.
        return ans;
    }
};
```