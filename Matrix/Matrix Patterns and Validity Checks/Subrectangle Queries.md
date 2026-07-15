**Question**: [Check if Matrix is X-Matrix](https://leetcode.com/problems/subrectangle-queries/)

**Approach**:
- You have 2 options, either brute force it in `updateSubrectangle` or store the changes
- If you store, go through the changes in reverse iteration in `getValue` function, and return the latest change.

TC: `O(change)` for `getValue` function
SC: `O(m*n)` for `v`, and storing changes

**Code**:
```cpp
class SubrectangleQueries {
public:
    vector<vector<int>>v;
    vector<tuple<int, int, int, int, int>>change;
    SubrectangleQueries(vector<vector<int>>& rectangle) {
        v = rectangle;
    }
    
    void updateSubrectangle(int row1, int col1, int row2, int col2, int newValue) {
        change.push_back({row1, col1, row2, col2, newValue});
    }
    
    int getValue(int row, int col) {
        // Get the latest change
        for(int i = change.size() - 1 ; i >= 0 ; i--){
            auto [row1, col1, row2, col2, newValue] = change[i];
            if(row >= row1 && col >= col1 && row <= row2 && col <= col2)    return newValue;
        }
        return v[row][col];
    }
};
```