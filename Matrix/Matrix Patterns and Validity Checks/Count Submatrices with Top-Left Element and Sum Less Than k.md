**Question**: [Count Submatrices with Top-Left Element and Sum Less Than k](https://leetcode.com/problems/count-submatrices-with-top-left-element-and-sum-less-than-k/)

**Approach**:
For each element, calculate the sum of submatrix.
The sum will be: `prev(row element) + prev(col element) - prev(diagonal element) + current element`

TC: `O(m*n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int countSubmatrices(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                // each element now actually has sum of submatrix
                grid[i][j] = (i>0 ? grid[i-1][j] : 0) + 
                            (j>0 ? grid[i][j-1] : 0) - 
                            (i>0 && j > 0 ? grid[i-1][j-1] : 0)
                            + grid[i][j];
                if(grid[i][j] <= k) ans++;
            }
        }

        return ans;
    }
};
```