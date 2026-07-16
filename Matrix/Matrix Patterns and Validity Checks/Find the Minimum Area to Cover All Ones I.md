**Question**: [Find the Minimum Area to Cover All Ones I](https://leetcode.com/problems/find-the-minimum-area-to-cover-all-ones-i/)

**Approach**:
- Just find maximum and minimum values of row, col whenever grid has 1.

TC: `O(m*n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int minimumArea(vector<vector<int>>& grid) {
        // find smallest and largest col, row
        int m = grid.size(), n = grid[0].size();
        int rowMin = m, rowMax = 0;
        int colMin = n, colMax = 0;
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                if(grid[i][j] == 1){
                    rowMin = min(rowMin, i), rowMax = max(rowMax, i);
                    colMin = min(colMin, j), colMax = max(colMax, j);
                }
            }
        }

        int area = (rowMax - rowMin + 1) * (colMax - colMin + 1);
        return area;
    }
};
```
