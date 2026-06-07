**Question**: [Sum of Absolute Differences in a Sorted Array](https://leetcode.com/problems/sum-of-absolute-differences-in-a-sorted-array/)

**Approach**:
For each index, we need to calculate the sum before it, and sum after it.
since this is a nondescreasing array, that means, `idx` > `i` will be greater
so, $ |nums[idx] - nums[i]| = nums[idx] - nums[i] $.
Similarily $|nums[idx] - nums[i]| = nums[i] - nums[idx]$ for `idx` < `i`.
so Sum of absolute difference of indices `idx` < `i` will be $n*nums[i]$ - sum of the rest of elements. 

TC: `O(n)`
SC: `O(n)`

**Code**:
```cpp
class Solution {
public:
    vector<int> getSumAbsoluteDifferences(vector<int>& nums) {
        int n = nums.size();
        vector<int>ans(n, 0);
        vector<int>pref(n+1, 0);
        //[___i______] -> x*i - sum of 0 to x elements + sum (x+1 to n) - (n-x)*(i)
        for(int i = 1 ; i <= n ; i++){
            pref[i] = pref[i-1] + nums[i-1];
        }
        for(int i = 0 ; i < n ; i++){
            ans[i] = pref[n] - pref[i] -(n-i)*nums[i] + i*(nums[i]) - pref[i];
        }

        return ans;
    }
};
```