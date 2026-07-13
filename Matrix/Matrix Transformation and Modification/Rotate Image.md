**Question**: [Rotate Image](https://leetcode.com/problems/rotate-image/)

**Approach**:
First transpose, then reverse each row

TC: $O(n^2)$
SC: $O(1)$
**Code**:
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        // We can do this by first transposing the array, then reversing rows
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < i ; j++){
                swap(matrix[i][j], matrix[j][i]);
            }
        }
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < n/2 ; j++){
                swap(matrix[i][j], matrix[i][n-j-1]);
            }
        }
    }
};
```