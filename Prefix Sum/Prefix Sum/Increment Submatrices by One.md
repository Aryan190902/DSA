**Question**: [Increment Submatrices by One](https://leetcode.com/problems/increment-submatrices-by-one/solutions/)

**Approach**: 
We use difference array technique in this, suppose you have a 1D array, and you want to apply sum operations between `l` and `r` i.e, `arr[i]` + 1, where `l <= i <= r`.
Instead of going through all the indices, we can just increment the start point (`l`), and decrement the index after the end, i.e., `r+1`.
If we want the sum of elements between `l` and `r` after all the operations, then after using the difference technique, we can sum all the elements => `arr[i] = arr[i-1] + arr[i]`. The `r` index will hold the sum.

Using the same approach in 2D array, we will just need to update all the rows instead of just one index, i.e, `arr[i][col] += 1`, where `r1 <= i <= r2`.

TC: $O(qn + n^2)$
SC: $O(1)$ [ Not considering the answer array space ]

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> rangeAddQueries(int n, vector<vector<int>>& queries) {
        vector<vector<int>>mat(n, vector<int>(n, 0));
        for(vector<int>&cord : queries){
            int r1 = cord[0], c1 = cord[1], r2 = cord[2], c2 = cord[3];
            for(int i = r1 ; i <= r2 ; i++) mat[i][c1] += 1;
            if(c2 < n-1){
                for(int i = r1 ; i <= r2 ; i++) mat[i][c2+1] -= 1;
            }
        }
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < n ; j++){
                mat[i][j] = (j > 0 ? mat[i][j-1] : 0) + mat[i][j];
            }
        }

        return mat;
    }
};
```

### Optimized Approach
**Approach**:
We are incrementing each row, so we are incrementing between range `r1`, and `r2`, this can also be solved using difference array

| Position	| Meaning |
 ---------- | ----------
| diff[r1][c1] += 1	| start adding +1 in this region | 
| diff[r2+1][c1] -= 1 |	stop vertical propagation |
| diff[r1][c2+1] -= 1 |	stop horizontal propagation |
| diff[r2+1][c2+1] += 1 |	cancel the double negative |

We can now modify the calculation to: `mat[i][j] = diff[i][j]
            + mat[i-1][j]
            + mat[i][j-1]
            - mat[i-1][j-1]`

TC: $O(q + n^2)$
SC: $O(n^2)$

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> rangeAddQueries(int n, vector<vector<int>>& queries) {
        vector<vector<int>>mat(n, vector<int>(n, 0));
        vector<vector<int>>diff(n, vector<int>(n, 0));
        for(vector<int>&cord : queries){
            int r1 = cord[0], c1 = cord[1], r2 = cord[2], c2 = cord[3];
            diff[r1][c1] += 1;
            if(r2 < n-1)    diff[r2 + 1][c1] -= 1;
            if(c2 < n-1)    diff[r1][c2 + 1] -= 1;
            if(r2 < n-1 && c2 < n-1)    diff[r2+1][c2+1] += 1; // cancel the double negative
        }

        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < n ; j++){
                mat[i][j] += diff[i][j];
                mat[i][j] += (i > 0) ? mat[i-1][j] : 0;
                mat[i][j] += (j > 0) ? mat[i][j-1] : 0;
                mat[i][j] -= (i > 0 && j > 0 ) ? mat[i-1][j-1] : 0; // Counted twice, so remove once.
            }
        }

        return mat;
    }
};
```
