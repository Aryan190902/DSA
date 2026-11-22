**Question**: [Subsets](https://leetcode.com/problems/subsets/)

**Approach**:
- At each index, make two choices: exclude `nums[idx]` or include it in the current subset.
- Passing `temp` by value keeps the current subset isolated in each recursive branch, so no explicit backtracking removal is needed.
- After every element has been processed, `temp` represents one complete subset and is added to the answer.
- For $n$ elements, the two recursive branches generate exactly $2^n$ subsets, including the empty subset.

TC: $O(n \times 2^n)$, since there are $2^n$ subsets and copying a subset when it is added may take up to $O(n)$.

SC: $O(n)$ auxiliary space for the recursion depth and temporary vectors; output space for the $2^n$ subsets is $O(n \times 2^n)$.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<int>& v, vector<vector<int>>& ans, int idx, vector<int> temp) {
        // All elements have been decided, so temp is one complete subset.
        if (idx == v.size()) {
            ans.push_back(temp);
            return;
        }

        // Exclude the current element and move to the next index.
        solve(v, ans, idx + 1, temp);

        // Include the current element, then decide the remaining elements.
        temp.push_back(v[idx]);
        solve(v, ans, idx + 1, temp);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;

        // Start with an empty subset at index zero.
        solve(nums, ans, 0, {});
        return ans;
    }
};
```