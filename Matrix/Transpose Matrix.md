**Question**: [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/)

**Approach**:
Very simple logic

TC: $O(n^2)$
SC: $O(1)$ not counting the ans 2D vector

**Code**:

```cpp
class Solution {
public:
    vector<vector<int>> transpose(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>>ans(n, vector<int>(m));
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m ; j++){
                ans[i][j] = matrix[j][i];
            }
        }
        return ans;
    }
};
```