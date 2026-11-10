**Question**: [3Sum](https://leetcode.com/problems/3sum/)

**Approach**:
- Sort the array so we can use the two-pointer technique efficiently.
- Fix one element `nums[i]` and treat the remaining problem as a two-sum on the subarray `nums[i+1..n-1]`.
- Use two pointers `j` and `k` from the ends of the remaining part.
- If the sum is too large, move `k` left; if it is too small, move `j` right.
- When a valid triplet is found, store it and skip duplicate values to avoid duplicates in the answer.

TC: `O(n^2)` because for each fixed element we run a linear two-pointer scan.
SC: `O(1)` extra space excluding the output, since sorting is in-place.

**Code**:
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> ans;

        sort(nums.begin(), nums.end()); // Sort to enable the two-pointer approach

        for (int i = 0; i < n; i++) {
            int x = -nums[i]; // Fix nums[i], now solve for nums[j] + nums[k] = -nums[i]
            int j = i + 1, k = n - 1;

            while (j < k) {
                int sm = nums[j] + nums[k];

                if (sm > x) {
                    k--; // Sum is too large, move the right pointer left
                } else if (sm < x) {
                    j++; // Sum is too small, move the left pointer right
                } else {
                    ans.push_back({nums[i], nums[j], nums[k]}); // Found a valid triplet
                    j++;

                    // Skip duplicate values for j to avoid repeated triplets
                    while (j < k && nums[j] == nums[j - 1]) j++;
                    // We do not skip k here because the next value of j is enough to avoid duplicate triplets
                }
            }

            // Skip duplicate values for the fixed element i
            while (i < n - 1 && nums[i] == nums[i + 1]) i++;
        }

        return ans;
    }
};
```