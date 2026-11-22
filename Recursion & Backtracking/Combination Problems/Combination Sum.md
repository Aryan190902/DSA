**Question**: [Combination Sum](https://leetcode.com/problems/combination-sum/)

**Approach**:
- Sort the candidates so recursion can stop early when `c[i] > t`.
- Backtrack from index `idx`, allowing each candidate to be reused while preventing permutations and duplicate combinations.
- Push a candidate into `temp`, recurse with `t - c[i]`, and keep the same index `i` so the candidate may be reused.
- Pop the candidate after the recursive call to restore the state for the next choice.
- If the remaining target becomes zero, append the complete combination to `ans`.

TC: $O(n \log n + N \times K)$, where $N$ is the number of valid recursive combinations explored and $K$ is the maximum length of a combination. Sorting takes $O(n \log n)$, and copying each combination into the answer takes up to $O(K)$.

SC: $O(K)$ auxiliary recursion and temporary storage, excluding the output size.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<int>& c, int t, vector<vector<int>>& ans,
               vector<int>& temp, int idx) {
        // The current temp is a valid combination for the original target.
        if (t == 0) {
            ans.push_back(temp);
            return;
        }

        // No candidates remain to complete the target.
        if (idx == c.size()) return;

        // Start at idx so candidates earlier than idx are never reconsidered.
        for (int i = idx; i < c.size(); i++) {

            // Candidates are sorted, so all later candidates will also be too large.
            if (c[i] <= t) {
                temp.push_back(c[i]); // Choose c[i] for the current combination.

                // Keep i unchanged because c[i] may be reused.
                solve(c, t - c[i], ans, temp, i);

                temp.pop_back(); // Undo the choice before trying the next candidate.
            } else {
                break;
            }
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        // Sorting enables the early break for candidates larger than the target.
        sort(candidates.begin(), candidates.end());

        vector<vector<int>> ans;
        vector<int> temp;
        solve(candidates, target, ans, temp, 0);

        return ans;
    }
};
```
