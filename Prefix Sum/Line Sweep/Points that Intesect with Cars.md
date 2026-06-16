**Question**: [Points that Intersect with Cars](https://leetcode.com/problems/points-that-intersect-with-cars/)

**Approach**: 
Contraints says value of `nums` integers are between `1` and `100`, so we create a vector of length `102`.
We loop through the `nums` array, and increment the first point, and decrement the point after the last, to note the changes in number of cars at the point.
We again loop through `cars` array to calculate the total cars at each point, and increment the answer if there is atleast 1 car.

TC: `O(n)`
SC: `O(102)` which is a constant, so ~`O(1)`

**Code**:
```cpp
class Solution {
public:
    int numberOfPoints(vector<vector<int>>& nums) {
        vector<int>cars(102, 0);
        for(vector<int>&v : nums){
            cars[v[0]]++; // car present
            cars[v[1] + 1]--; // car reset
        }
        int ans = 0;
        for(int i = 0 ; i < 102 ; i++){
            cars[i] += i > 0 ? cars[i-1] : 0; // total cars in that spot
            ans += cars[i] > 0; // if car present
        }

        return ans;
    }
};
```