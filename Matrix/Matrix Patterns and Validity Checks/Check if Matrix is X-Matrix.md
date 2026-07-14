**Question**: [Check if Matrix is X-Matrix](https://leetcode.com/problems/check-if-matrix-is-x-matrix/)

**Approach**:
If diagonal, it is non-zero, else zero.

TC: `O(m*n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    bool checkXMatrix(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                if(i == j){
                    if(grid[i][j] == 0) return false;
                }
                else if(i == n - j - 1){
                    if(grid[i][j] == 0) return false;
                }
                else{
                    if(grid[i][j] != 0) return false;
                }
            }
        }
        return true;
    }
};
```