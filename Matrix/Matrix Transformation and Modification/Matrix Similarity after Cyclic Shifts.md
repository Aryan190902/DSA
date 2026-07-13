**Question**: [Matrix Similarity after Cyclic Shifts](https://leetcode.com/problems/matrix-similarity-after-cyclic-shifts/)

**Approach**:
Create the resultant array by shifting k times left and right as required, and check the original and resultant array.

TC: $O(n^2)$
SC: $O(n^2)$

**Code**:
```cpp
class Solution {
public:

    void shift(vector<int>&v, int k, char dir){
        int n = v.size();
        reverse(v.begin(), v.end());
        if(dir == 'l'){
            reverse(v.begin() + n-k, v.end());
            reverse(v.begin(), v.begin() + n-k);
        }else{
            reverse(v.begin(), v.begin()+k);
            reverse(v.begin()+k, v.end());
        }
    }

    bool areSimilar(vector<vector<int>>& mat, int k) {
        vector<vector<int>>v = mat;
        int m = v.size(), n = v[0].size();
        // for even, left shift k times, and right shift k times for odd
        k %= n;
        for(int i = 0 ; i < m ; i++){
            if(k%2 == 0)    shift(v[i], k, 'l');
            else    shift(v[i], k, 'r');
        }
        return mat == v;
    }
};
```

### Optimized Solution

We don't need to actually create the resultant array at all, since we know the shifts for each row, we can directly check if old and new Idx values match or not.

TC: $O(n^2)$
SC: $O(1)$

```cpp
class Solution {
public:
    bool areSimilar(vector<vector<int>>& mat, int k) {
        int m = mat.size(), n = mat[0].size();
        k %= n;
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                if(i&1){
                    if(mat[i][j] != mat[i][(j-k+n)%n])  return false;
                }else{
                    if(mat[i][j] != mat[i][(j+k)%n])    return false;
                }
            }
        }

        return true;
    }
};
```