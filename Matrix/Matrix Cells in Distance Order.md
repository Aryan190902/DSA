**Question**: [Matrix Cells in Distance Order](https://leetcode.com/problems/matrix-cells-in-distance-order/)

**Approach**:
Brute force approach where you generate all the elements of a matrix, and sort them accordingly.

TC: `O(nlogn)` where n = rows*cols
SC: `O(n)` which is just the ans array.

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> allCellsDistOrder(int rows, int cols, int rCenter, int cCenter) {
        vector<vector<int>>ans;
        for(int i = 0 ; i < rows ; i++){
            for(int j = 0 ; j < cols ; j++){
                ans.push_back({i, j});
            }
        }
        sort(ans.begin(), ans.end(), [&](auto &a, auto &b){
            return abs(a[0] - rCenter) + abs(a[1] - cCenter) < 
                    abs(b[0] - rCenter) + abs(b[1] - cCenter);
        });

        return ans;
    }
};
```

#### Optimal Solution

We can further decrease the time complexity to `O(n)` using BFS
TC: `O(n)`
```cpp
class Solution {
public:
    vector<vector<int>> allCellsDistOrder(int rows, int cols, int rCenter, int cCenter) {
        vector<vector<int>>dir{{0,1}, {1,0}, {-1,0}, {0,-1}};
        vector<vector<int>>vis(rows, vector<int>(cols, 0));
        vector<vector<int>>ans;

        queue<pair<int, int>>q;
        q.push({rCenter, cCenter});
        vis[rCenter][cCenter] = true;

        while(!q.empty()){
            auto temp = q.front();
            q.pop();
            int x = temp.first, y = temp.second;
            ans.push_back({x, y});
            for(int i = 0 ; i < 4 ; i++){
                int r = x + dir[i][0];
                int c = y + dir[i][1];
                if(r >= 0 && c >= 0 && r < rows && c < cols && !vis[r][c]){
                    q.push({r, c});
                    vis[r][c] = true;
                }
            }
        }
        return ans;

    }
};
```