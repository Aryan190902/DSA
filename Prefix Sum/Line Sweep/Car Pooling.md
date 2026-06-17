**Question**: [Car Pooling](https://leetcode.com/problems/car-pooling/)

**Approach**: 
Using line sweep, we can find the number of passengers at each km, just need to add a capacity condition at the end,

TC: `O(n)`
SC: `O(1001)` which is a constant, so ~`O(1)`

```cpp
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int>passn(1001, 0); // 0 < trip <= 1000
        int n = trips.size();
        for(int i = 0 ; i < n ; i++){
            int psgers = trips[i][0];
            int l = trips[i][1], r = trips[i][2];
            passn[l] += psgers;
            if(r < 1001)   passn[r] -= psgers;
        }
        for(int i = 0 ; i < 1001 ; i++){
            passn[i] += i > 0 ? passn[i-1] : 0;
            if(passn[i] > capacity) return false;
        }

        return true;
    }
};
```