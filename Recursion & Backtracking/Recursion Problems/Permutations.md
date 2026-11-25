**Question**: [Permutations](https://leetcode.com/problems/permutations/)

**Approach**:

- Use backtracking to build each permutation one position at a time in `temp`.
- `vis[i]` tracks whether `nums[i]` is already included in the current permutation, ensuring each index is used exactly once.
- At every position, try every unused element, mark it as visited, recurse, then undo both choices before trying the next element.
- When `temp.size()` equals `nums.size()`, all positions have been filled and the current permutation can be added to `ans`.

TC: $O(n \cdot n!)$, since the recursion explores $n!$ permutations and each complete permutation requires $O(n)$ time to copy into `ans`.

SC: $O(n)$ auxiliary space for `temp`, `vis`, and the recursion stack, excluding the $O(n \cdot n!)$ space used for the returned output.

**Code**:
```cpp
class Solution {
public:
    void solve(vector<int>& nums, vector<vector<int>>& ans,
               vector<int>& temp, vector<int>& vis) {
        // A complete permutation contains exactly one copy of every input index.
        if (temp.size() == nums.size()) {
            ans.push_back(temp);
            return;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (!vis[i]) {
                // Fix nums[i] at the next position in the permutation.
                vis[i] = 1;
                temp.push_back(nums[i]);

                solve(nums, ans, temp, vis);

                // Undo the choice so the next unused element can be tried.
                temp.pop_back();
                vis[i] = 0;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> temp;
        vector<int> vis(nums.size(), 0);

        solve(nums, ans, temp, vis);
        return ans;
    }
};
```