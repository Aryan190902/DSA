**Question**: [Queens That Can Attack the King](https://leetcode.com/problems/queens-that-can-attack-the-king/)

**Approach**:
- Start from the king
- For each direction check if a queen is present or not
- If present, push the position in `ans` and break the loop.

TC: `O(64)` ~ `O(1)`
SC: `O(64)` -> board and ans arrays

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> queensAttacktheKing(vector<vector<int>>& queens, vector<int>& king) {
        vector<vector<int>>board(8, vector<int>(8, 0));
        vector<vector<int>>ans;
        for(vector<int>&queen : queens){
            int x = queen[0], y = queen[1];
            board[x][y] = 1;
        }
        int x = king[0], y = king[1];
        for(int dx = -1 ; dx <= 1 ; dx++){
            for(int dy = -1 ; dy <= 1 ; dy++){
                if(dx == 0 && dy == 0)  continue;
                x = king[0];
                y = king[1];
                int tmpX = x+dx, tmpY = y+dy;
                while(tmpX >= 0 && tmpX < 8 && tmpY >= 0 && tmpY < 8){
                    if(board[tmpX][tmpY] == 1){
                        ans.push_back({tmpX, tmpY});
                        break;
                    }
                    tmpX += dx;
                    tmpY += dy;
                }
            }
        }

        return ans;
    }
};
```