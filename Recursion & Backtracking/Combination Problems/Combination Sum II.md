**Question**: [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

**Approach**:

- Sort candidates first so duplicates are adjacent and early pruning works.
- Use backtracking from index `idx` and try each candidate `c[i]` starting at `idx`.
- Skip duplicate candidates at the same recursion level using `if(i > idx && c[i] == c[i-1]) continue`.
- Break the loop when `c[i] > t` because further candidates are too large.
- Push `c[i]`, recurse with remaining target `t-c[i]` and next index `i+1`, then pop back.
- Add `temp` to `ans` when `t == 0`.

TC: `O(2^n * n)` in the worst case due to backtracking and combination copying.
SC: `O(n)` recursion depth plus output space.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<int>&c, int t, vector<vector<int>>&ans, vector<int>&temp, int idx){
        if(t == 0){
            ans.push_back(temp);
            return;
        }
        for(int i = idx ; i < c.size() ; i++){
            if(i > idx && c[i] == c[i-1])   continue;
            if(c[i] > t)  break;
            temp.push_back(c[i]);
            solve(c, t-c[i], ans, temp, i+1);
            temp.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<int>temp;
        vector<vector<int>>ans;
        sort(candidates.begin(), candidates.end());
        solve(candidates, target, ans, temp, 0);

        return ans;
    }
};
```