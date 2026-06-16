**Question**: [Maximum Population Year](https://leetcode.com/problems/maximum-population-year/)

**Approach**: 
Since the constraints say birth and death are between 1950, and 2050, we can assume an vector of length 100.
For each year, we calculate the population changes, then just use a for loop to get the pop of each year.

TC: `O(n)`
SC: `O(100)` which is a constant, so ~`O(1)`

**Code**:
```cpp
class Solution {
public:
    int maximumPopulation(vector<vector<int>>& logs) {
        vector<int>popInCurrentYear(101, 0); // constraints shows 100 year diff
        int startYear = 1950; // min value in logs
        for(vector<int>&l : logs){
            int b = l[0]-startYear, d = l[1]-startYear;
            popInCurrentYear[b]++; // pop inc by 1 in this year
            popInCurrentYear[d]--; // pop dec by 1 in this year
        }
        int ans = 0;
        for(int i = 0 ; i < 100 ; i++){
            popInCurrentYear[i] += i > 0 ? popInCurrentYear[i-1] : 0; // total pop this year
            ans = popInCurrentYear[i] > popInCurrentYear[ans] ? i : ans;
        }
        ans += startYear;
        return ans;
    }
};
```