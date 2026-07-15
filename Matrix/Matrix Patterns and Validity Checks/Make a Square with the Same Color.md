**Question**: [Make a Square with the Same Color](https://leetcode.com/problems/make-a-square-with-the-same-color/)

**Approach**:
- Only need to traverse the 4 2x2 square present in the 3x3 grid.


TC: `O(2*2*4)` ~ `O(1)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    bool canMakeSquare(vector<vector<char>>& grid) {
        auto isW = [](char c){
            return c == 'W' ? 1 : 0;
        };
        vector<int>a{0,0,1,1};
        vector<int>b{0,1,0,1};
        for(int i = 0 ; i < 2 ; i++){
            for(int j = 0 ; j < 2 ; j++){
                // loop through all 4 elements
                int x = 0;
                for(int k = 0 ; k < 4 ; k++){
                    x += isW(grid[i+a[k]][j+b[k]]);
                }
                if(x != 2)    return true;
            }
        }
        return false;
    }
};
```