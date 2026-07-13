**Question**: [Convert 1D array to 2D array](https://leetcode.com/problems/convert-1d-array-to-2d-array/)

**Approach**:
Just do as asked.

TC: `O(n*m)`
SC: `O(n*m)` which is just the res array.

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> construct2DArray(vector<int>& original, int m, int n) {
        if(m*n != original.size())  return {};
        int idx = 0;
        vector<vector<int>>res(m, vector<int>(n, 0));
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                res[i][j] = original[idx++];
            }
        }

        return res;
    }
};
```