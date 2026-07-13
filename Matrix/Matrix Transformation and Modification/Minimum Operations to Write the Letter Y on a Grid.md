**Question**: [Minimum Operations to Write the Letter Y on a Grid](https://leetcode.com/problems/minimum-operations-to-write-the-letter-y-on-a-grid/description/)

**Approach**:
We can start by understanding the distribution of 0,1,2 in matrix, in Y and non-Y elements.
Y elements will be the upper diagonal elements, and middle col elements, rest will be non-Y.
Lastly, check for all the possible conditions.

TC: $O(n^2)$
SC: $O(1)$, since vectors have constant size
**Code**:
```cpp
class Solution {
public:
    int minimumOperationsToWriteY(vector<vector<int>>& grid) {
        int n = grid.size();
        auto isPartofY = [n](int row, int col){
            if(row >= n/2)  return col == n/2;
            bool res = (row == col) || (row == (n - col - 1));
            return res;
        };
        vector<int>y(3, 0), nonY(3, 0);

        // O(n^2)
        for(int i = 0 ; i < n ; i++){
            for(int j = 0 ; j < n ; j++){
                if(isPartofY(i, j)){
                    y[grid[i][j]]++;
                }else{
                    nonY[grid[i][j]]++;
                }
            }
        }
        int yEle = 3*(n/2) + 1;
        /*
            in Y, total elements will be counted by:
            - n/2 from left diagonal
            - n/2 from right diagonal
            - n/2 from middle to bottom
            - 1 center elements
        */

        // check all the cases, y=0, nonY = 1 or 2, etc
        int nonYele = n*n - yEle;
        int res = yEle - y[0] + nonYele - max(nonY[1], nonY[2]);
        res = min(res, yEle - y[1] + nonYele - max(nonY[0], nonY[2]));
        res = min(res, yEle - y[2] + nonYele - max(nonY[0], nonY[1]));

        return res;
    }
};
```