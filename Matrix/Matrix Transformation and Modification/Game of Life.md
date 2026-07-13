**Question**: [Game of Life](https://leetcode.com/problems/game-of-life/)

**Approach**:
- For each index, find the number of live neighbours.
- Change the value according to the conditions given.

TC: `O(n*m)`
SC: `O(n*m)`

**Code**:
```cpp
class Solution {
public:
    int getLiveCells(vector<vector<int>>&grid, int x, int y){
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        vector<int>xChange{-1,-1,-1,0,0,1,1,1};
        vector<int>yChange{-1,0,1,1,-1,-1,0,1};
        for(int i = 0 ; i < 8 ; i++){
            int newX = xChange[i] + x ;
            int newY = yChange[i] + y ;
            if(newX >= 0 && newX < m && newY >= 0 && newY < n){
                ans += grid[newX][newY] == 1;
            }
        }
        return ans;
    }

    void gameOfLife(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>>newState(grid.begin(), grid.end());
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                int liveCells = getLiveCells(grid,i,j);
                if(liveCells < 2 || liveCells > 3)  newState[i][j] = 0;
                else if(liveCells >= 2 && liveCells < 3)   continue;
                else if(liveCells == 3) newState[i][j] = 1;
            }
        }

        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                grid[i][j] = newState[i][j];
            }
        }
    }
};
```

#### Optimized Solution

We can decrease the space complexity by encoding the array to show the state as well as previous value
for example:

| Encode | Meaning        |
| -------|----------------|
| 00     | dead->dead     |
| 01     | alive -> dead  |
| 10     | dead -> alive  | 
| 11     | alive -> alive |

New SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int getLiveCells(vector<vector<int>>&grid, int x, int y){
        int m = grid.size(), n = grid[0].size();
        int ans = 0;
        vector<int>xChange{-1,-1,-1,0,0,1,1,1};
        vector<int>yChange{-1,0,1,1,-1,-1,0,1};
        for(int i = 0 ; i < 8 ; i++){
            int newX = xChange[i] + x ;
            int newY = yChange[i] + y ;
            if(newX >= 0 && newX < m && newY >= 0 && newY < n){
                ans += grid[newX][newY] & 1; // only checking the last bit (previous state)
            }
        }
        return ans;
    }

    void gameOfLife(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        // store the next state in 2nd bit
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                int liveCells = getLiveCells(grid,i,j);
                if(grid[i][j] & 1){ // currently alive
                    if(liveCells == 2 || liveCells == 3){
                        grid[i][j] |= 2; // set 2nd bit, since 2 -> 10, grid[i][j] | 2 will set the 2nd bit
                    }
                }
                else{ // Currently dead
                    if(liveCells == 3){
                        grid[i][j] |= 2;
                    }
                }
            }
        }
        // Update the board to the next state
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                grid[i][j] >>= 1; // shift bits to right once 
            }
        }
    }
};
```
