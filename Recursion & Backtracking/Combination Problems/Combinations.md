**Question**: [Combinations](https://leetcode.com/problems/combinations/)

**Approach**:

- Use backtracking (DFS) with a `start` parameter to ensure increasing choices and avoid duplicates.
- Pass `temp` by reference to avoid repeated copying; push a candidate, recurse with `start = i+1`, then pop to backtrack.
- Prune the loop with `upper_bound = n - (k - temp.size()) + 1` so the loop only runs while enough numbers remain to fill the combination.
- When `temp.size() == k`, append `temp` to `ans` and return.

TC: `O(C(n, k) * k)` — generate all combinations `C(n,k)`, each copied in O(k). Pruning reduces constant work.
SC: `O(k)` recursion depth plus output space `O(C(n, k) * k)`.

**Code**:
```cpp
class Solution {
public:
    void solve(int n, int k, vector<vector<int>>& ans, vector<int>& temp, int start) {
        if (temp.size() == k) {
            ans.push_back(temp);
            return;
        }
        
        // Prune search space: only loop while enough elements remain
        int upper_bound = n - (k - temp.size()) + 1;
        for (int i = start; i <= upper_bound; i++) {
            temp.push_back(i);
            solve(n, k, ans, temp, i + 1);
            temp.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> temp;
        solve(n, k, ans, temp, 1);
        return ans;
    }
};
```