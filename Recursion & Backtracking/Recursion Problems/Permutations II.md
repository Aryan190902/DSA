**Question**: [Permutations II](https://leetcode.com/problems/permutations-ii/)

**Approach**:

- Sort `nums` so duplicate values are adjacent.
- Use backtracking with `vis[i]` to track which array positions are currently included in `temp`.
- At each position, skip a duplicate index when the previous duplicate is unused. Thus, among equal values, the first unused duplicate is always chosen before the later one.
- When `temp.size()` equals `nums.size()`, add the unique permutation to `ans` and return.

TC: $O(n \cdot n!)$ in the worst case, where the result contains $n!$ permutations and copying each permutation takes $O(n)$ time. The duplicate check avoids generating the duplicate permutations.

SC: $O(n)$ auxiliary space for `temp`, `vis`, and the recursion stack, excluding the output space.

**Code**:
```cpp
class Solution {
public:
    void solve(vector<int>& nums, vector<vector<int>>& ans,
               vector<int>& temp, vector<int>& vis) {
        // Every index has been placed exactly once, so temp is a permutation.
        if (temp.size() == nums.size()) {
            ans.push_back(temp);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (!vis[i]) {
                // nums is sorted, so equal values are adjacent.
                // Skip this duplicate while the earlier duplicate is still unused;
                // this ensures equal values are fixed in a stable order.
                if (i > 0 && nums[i] == nums[i - 1] && !vis[i - 1]) {
                    continue;
                }

                vis[i] = 1;
                temp.push_back(nums[i]);

                solve(nums, ans, temp, vis);

                // Undo the choice so the remaining indices can be tried.
                temp.pop_back();
                vis[i] = 0;
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        // Sorting places duplicate values together for the pruning check.
        sort(nums.begin(), nums.end());

        vector<vector<int>> ans;
        vector<int> temp;
        vector<int> vis(nums.size(), 0);

        solve(nums, ans, temp, vis);
        return ans;
    }
};
```