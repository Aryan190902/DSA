**Question**: [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

**Approach**:
- Create 2 arrays to note what rows, and cols need to be 0. 
- Set them zero.

TC: $O(nm)$ -> Looping through matrix
SC: $O(n)+O(m)$ -> Arrays

**Code**:
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<int>rows(n, 0), cols(m, 0);
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m ; j++){
                if(matrix[i][j] == 0){
                    rows[i] = 1;
                    cols[j] = 1;
                }
            }
        }
        // rows that will be 0
        for(int i = 0 ; i < n ; i++){
            if(rows[i]){
                for(int j = 0 ; j < m ; j++){
                    matrix[i][j] = 0;
                }
            }
        }

        // cols that will be 0
        for(int j = 0 ; j < m ; j++){
            if(cols[j]){
                for(int i = 0 ; i < n ; i++){
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```