**Question**: [Modify the Matrix](https://leetcode.com/problems/modify-the-matrix/)

**Approach**:
Do as told.

TC: `O(n*m)`
SC: `O(n)` worst case: if all values of a column are -1.

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> modifiedMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        for(int j = 0 ; j < m ; j++){
            int mxx = -2;
            vector<int>negOnes;
            for(int i = 0 ; i < n ; i++){
                mxx = max(mxx, matrix[i][j]);
                if(matrix[i][j] == -1)  negOnes.push_back(i);
            }
            for(int idx : negOnes){
                matrix[idx][j] = mxx;
            }
        }
        return matrix;
    }
};
```