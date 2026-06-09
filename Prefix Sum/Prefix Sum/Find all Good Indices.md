**Question**: [Find all Good Indices](https://leetcode.com/problems/find-all-good-indices/)

**Approach**:
We calculate the number of continuous non-increasing indices, and non-decreasing indices.
Then just check if it satisfies the conditions

TC: `O(n)`
SC: `O(n)`

**Code**:
```cpp
class Solution {
public:
    vector<int> goodIndices(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int>noninc(n, 1), nondec(n, 1);
        vector<int>ans;
        for(int i = 1 ; i < n ; i++){
            if(nums[i-1] >= nums[i])    noninc[i] = noninc[i-1]+1;
        }
        for(int i = n-2 ; i >= 0 ; i--){
            if(nums[i] <= nums[i+1])    nondec[i] = nondec[i+1]+1;
        }
        
        for(int i = k ; i < n-k ; i++){
            if(noninc[i] >= k && nondec[i] >= k)    ans.push_back(i);
        }

        return ans;
    }
};
```