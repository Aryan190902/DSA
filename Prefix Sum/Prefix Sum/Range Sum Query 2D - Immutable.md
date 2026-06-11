**Question**: [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

**Approach**:
- Create a 2D sum array named `pref`.
- `pref[i][j]` will denote the sum of rectangle with left corner at (0,0), and right corner at (i-1, j-1).
- Calculate the result by visualizing the equation.

TC: `O(1)` for `sumRegion` function, $O(n^2)$ overall
SC: $O(n^2)$

**Code**:
```cpp
class NumMatrix {
public:
    vector<vector<int>>pref;
    NumMatrix(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        pref.resize(n+1, vector<int>(m+1, 0));
        /*
        assuming pref[i][j] gives the sum 
        of a rectangle with (0,0) left corner and 
        (i-1, j-1) right corner 
        */
        for(int i = 1 ; i <= n ; i++){
            for(int j = 1 ; j <= m ; j++){
                // sum => sum(pref[left]) + sum(pref[up]) + diag elem - sum(pref[diag elem])
                // since elements in diag elem are repeated. 
                pref[i][j] = pref[i-1][j] + pref[i][j-1] + matrix[i-1][j-1] - pref[i-1][j-1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        // visualize the matrix, and solve it.
        return pref[row2+1][col2+1] - pref[row1][col2+1] - pref[row2+1][col1] + pref[row1][col1];
    }
};

```