
**Question**: [Two Sum](https://leetcode.com/problems/two-sum/)

**Approach**:
- Traverse the array once while storing each previously seen number and its index in a hash map.
- For each `nums[i]`, calculate the complement `target - nums[i]` and check whether it is already in the map.
- If it is, return the stored index and `i`. Otherwise, store the current number and its index.
- Check for the complement before inserting the current number, so an element cannot be used twice. This also handles duplicate values correctly.

TC: `O(n)` average, with hash map lookups and insertions.
SC: `O(n)` for the hash map.

**Code**:
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int>mp;
        int n = nums.size();
        mp.reserve(n);
        for(int i = 0 ; i < n ; i++){
            int need = target - nums[i];
            auto iter = mp.find(need);
            if(iter != mp.end())    return {iter->second, i};
            mp[nums[i]] = i;
        }
        return {};
    }
};
```