**Question**: [Rotating the Box](https://leetcode.com/problems/rotating-the-box/)

**Approach**:
- Observe that we are trying to shift each element to the right as much as possible.
- We need to keep in mind about obstacles too, that won't move, so we can set the new base as `obstacle - 1` index.
- After that just need to shift 90°, which will be transpose the array, and reverse each row.

TC: `O(n*m)`
SC: `O(n*m)`

**Code**:
```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {
        int m = boxGrid.size(), n = boxGrid[0].size();
        vector<vector<char>>ans(n, vector<char>(m));
        vector<vector<char>>bef(m, vector<char>(n, '.')); // before Rotation

        for(int i = 0 ; i < m ; i++){
            // for each row, move elements to the right
            int base = n-1;
            for(int j = n-1 ; j >= 0 ; j--){
                if(boxGrid[i][j] == '*'){
                    // obstacle
                    base = j-1;
                    bef[i][j] = '*';
                }else if(boxGrid[i][j] == '#'){
                    bef[i][base--] = '#';
                }
            }
        }
        // transpose
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                ans[j][i] = bef[i][j];
            }
        }

        // reverse each row
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < m/2 ; j++){
                swap(ans[i][j], ans[i][m-j-1]);
            }
        }

        return ans;
    }
};
```
#### Optimized Solution

We can also do it in a single pass, like this:
```cpp
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& boxGrid) {
        int m = boxGrid.size(), n = boxGrid[0].size();
        vector<vector<char>>ans(n, vector<char>(m, '.'));

        for(int i = 0 ; i < m ; i++){
            int base = n-1;
            for(int j = n-1 ; j >= 0 ; j--){
                if(boxGrid[i][j] == '*'){
                    // obstacle
                    base = j-1;
                    ans[j][m-i-1] = '*';
                }else if(boxGrid[i][j] == '#'){
                    ans[base--][m-i-1] = '#';
                }
            }
        }

        return ans;
    }
};
```