**Question**: [Max Increase to Keep City Skyline](https://leetcode.com/problems/max-increase-to-keep-city-skyline/)

**Approach**:
- Get max values of each row and col
- For each element, you can increase the value to `min(rowMax, colMax)`.

TC: `O(m*n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int>rowMax(m, 0), colMax(n, 0);
        for(int i = 0 ; i < m ; i++){
            int mx = rowMax[i];
            for(int j = 0 ; j < n ; j++){
                mx = max(mx, grid[i][j]);
            }
            rowMax[i] = mx;
        }
        for(int i = 0 ; i < n ; i++){
            int mx = colMax[i];
            for(int j = 0 ; j < m ; j++){
                mx = max(mx, grid[j][i]);
            }
            colMax[i] = mx;
        }
        int ans = 0;
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                int val = min(rowMax[i], colMax[j]);
                ans += val - grid[i][j];
            }
        }

        return ans;
    }
};
```