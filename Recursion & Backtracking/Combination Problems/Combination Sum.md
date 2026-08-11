**Question**: [Combination Sum](https://leetcode.com/problems/combination-sum/)

**Approach**:

- Sort the candidates so the recursion can break early when the remaining target is too small.
- Use backtracking from index `idx` to try each candidate starting from `idx`, allowing repetition of the same candidate.
- For each valid `c[i]`, push it into `temp`, recurse with reduced target `t-c[i]` and the same index `i`, then pop it back.
- If `t` becomes zero, add the current combination to `ans`.
- The search never moves backwards over used candidates, so combinations remain non-decreasing and duplicates are avoided naturally.

TC: `O(n log n + n^(target/min(candidates)) * target/min(candidates))` in the worst case.
SC: `O(target/min(candidates))` auxiliary space for recursion and `temp`, excluding output space.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<int>&c, int t, vector<vector<int>>&ans, vector<int>&temp, int idx){
        if(t == 0){
            ans.push_back(temp);
            return;
        }
        // pick or not pick
        for(int i = idx ; i < c.size() ; i++){ 
            // Start from i only, since we can pick the same index muliple times
            if(c[i] > t)    break; // Since array is sorted

            temp.push_back(c[i]);
            solve(c, t-c[i], ans, temp, i);
            temp.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>>ans;
        vector<int>temp;
        sort(candidates.begin(), candidates.end());
        int n = candidates.size();
        solve(candidates, target, ans, temp, 0);
        return ans;
    }
};
```
