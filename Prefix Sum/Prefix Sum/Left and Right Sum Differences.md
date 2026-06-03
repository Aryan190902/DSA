**Question**: [Left and Right Sum Differences](https://leetcode.com/problems/left-and-right-sum-differences/)

**Approach**:
We just need to find the prefix sum, and suffix sum. 
The answer will be the absolute difference.

TC: `O(n)`
SC: `O(n)`
**Code**:

```cpp
class Solution {
public:
    vector<int> leftRightDifference(vector<int>& nums) {
        int n = nums.size();
        vector<int>pref{0};
        vector<int>suff(n+1, 0);
        for(int i = 1 ; i <= n ; i++){
            pref.push_back(nums[i-1] + pref.back());
        }
        for(int i = n-2 ; i >= 0 ; i--){
            suff[i] = nums[i+1] + suff[i+1];
        }
        vector<int>ans;
        for(int i = 0 ; i < n ; i++){
            ans.push_back(abs(pref[i] - suff[i]));
        }

        return ans;
    }
};
```