**Question**: [Shift 2D Grid](https://leetcode.com/problems/shift-2d-grid/)

**Approach**:
Basically, just find the new index for each element and place them into a new 2D array.

TC: $O(n^2)$
SC: $O(n^2)$
**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int len = m * n;
        k %= len;
        vector<vector<int>> ans(m, vector<int>(n));
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                int oIdx = i*n + j; // rowIdx * cols + col
                int nIdx = (oIdx+k)%len; // shift it by k

                int newRow = nIdx/n;
                int newCol = nIdx%n;
                ans[newRow][newCol] = grid[i][j];
            }
        }
                

        return ans;
    }
};
```