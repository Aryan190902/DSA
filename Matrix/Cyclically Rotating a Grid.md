**Question**: [Cycically Rotating a Grid](https://leetcode.com/problems/cyclically-rotating-a-grid/)

**Approach**:
- For each layer, we perform rotation k times

TC: `O(n*m*k)`
SC: `O(m+n)` temp array

```cpp
class Solution {
public:
    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int layer = min(m,n)/2;
        for(int lay = 0 ; lay < layer ; lay++){
            vector<int>temp;
            int t = lay, l = lay;
            int b = m-lay-1, r = n-lay-1;
            
            // top row
            for(int j = l ; j <= r ; j++){
                temp.push_back(grid[t][j]);
            }
            // right col
            for(int i = t+1 ; i < b ; i++){
                temp.push_back(grid[i][r]);
            }
            // bottom row
            for(int j = r ; j >= l ; j--){
                temp.push_back(grid[b][j]);
            }
            // left col
            for(int i = b-1 ; i > t ; i--){
                temp.push_back(grid[i][l]);
            }
            int sz = temp.size();
            int st = k%sz;
            
            // now replace the values

            // top row
            for(int j = l ; j <= r ; j++){
                grid[t][j] = temp[st];
                st++;
                if(st == sz)    st = 0;
            }
            // right col
            for(int i = t+1 ; i < b ; i++){
                grid[i][r] = temp[st];
                st++;
                if(st == sz)    st = 0;
            }
            // bottom row
            for(int j = r ; j >= l ; j--){
                grid[b][j] = temp[st];
                st++;
                if(st == sz)    st = 0;
            }
            // left col
            for(int i = b-1 ; i > t ; i--){
                grid[i][l] = temp[st];
                st++;
                if(st == sz)    st = 0;
            }
        }

        return grid;
    }
};
```