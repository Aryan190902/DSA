**Question**: [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

**Approach**:
- Keep a running prefix sum `sm` while traversing the array.
- At each index, a previous prefix sum equal to `sm - k` means the elements between that prefix and the current index sum to `k`.
- Store the frequency of each prefix sum in a hash map. Add the frequency of `sm - k` to the answer, then record the current prefix sum.
- Initialize the map with `mp[0] = 1` to count subarrays that start at index `0`.
- This prefix-sum approach works even when the array contains negative numbers, unlike a simple sliding window.

TC: `O(n)` average.
SC: `O(n)` for the hash map.

**Code**:
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0;
        unordered_map<int, int>mp;
        mp[0] = 1;
        int sm = 0; // Running Sum
        for(int i = 0 ; i < n ; i++){
            sm += nums[i];
            if(mp.count(sm-k))    ans += mp[sm - k]; // If sm-k found, add the counts of it.
            mp[sm]++;
        }

        return ans;
    }
};
```