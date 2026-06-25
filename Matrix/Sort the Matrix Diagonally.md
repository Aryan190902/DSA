**Question**: [Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally/)

**Approach**:
- We start from bottom-left to top to top-right, and store diagonal elements in a vector.
- We sort that vector and update `mat`.

TC: $O(n*m*log(min(n,m)))$
SC: $O(min(n,m))$ At a time, temp will only have at most `min(n,m)` elements, ignoring `res` since we can just overwrite `mat`

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        vector<vector<int>>res(mat.begin(), mat.end());
        int n = mat.size(), m = mat[0].size();
        /*
            start from bottom left, go up, and then right
            create temp vector, sort them, place them in the res array
            return it
        */
        for(int i = n-1 ; i >= 0 ; i--){
            vector<int>temp;
            int tmpI = i, tmpJ = 0;
            while(tmpI < n && tmpJ < m){
                temp.push_back(mat[tmpI][tmpJ]);
                tmpI++, tmpJ++;
            }
            sort(temp.begin(), temp.end());
            tmpI = i, tmpJ = 0;
            for(int idx : temp){
                res[tmpI++][tmpJ++] = idx;
            }
        }
        for(int j = 1 ; j < m ; j++){
            vector<int>temp;
            int tmpI = 0, tmpJ = j;
            while(tmpI < n && tmpJ < m){
                temp.push_back(mat[tmpI][tmpJ]);
                tmpI++, tmpJ++;
            }
            sort(temp.begin(), temp.end());
            tmpI = 0, tmpJ = j;
            for(int idx : temp){
                res[tmpI++][tmpJ++] = idx;
            }
        }

        return res;
    }
};
```

### Other Approach (Less optimized, but a good learning)

**Approach**:
- Create a Map with priority queue as a value, each priority queue is a min heap that is used for sorting the diagonal
- `i-j` value will be constant in elements on the same diagonal, we can use that.

TC: $O(n*m*log(min(n,m)))$
SC: $O(n*m)$
**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        int n = mat.size(), m = mat[0].size();
        /*
            create a map with priority queue as val, insert diagonal elements
            update the diagonal elements
        */
        unordered_map<int, priority_queue<int, vector<int>, greater<int>>>mp;
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m ; j++){
                mp[i-j].push(mat[i][j]); // i-j in a diagonal will be same
                // since both i, and j increases by 1
            }
        }
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m ; j++){
                mat[i][j] = mp[i-j].top();
                mp[i-j].pop();
            }
        }

        return mat;
    }
};
```
