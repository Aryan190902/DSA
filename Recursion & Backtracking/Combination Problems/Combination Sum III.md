**Question**: [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

**Approach**:

- Use backtracking with `idx` tracking the next candidate number from 1 to 9.
- Stop when `n == 0`; if remaining target `t` is zero, record the current combination.
- For each `i` from `idx` to 9, break early if `i > t` because further numbers are too large.
- Push `i`, recurse with `t-i`, `n-1`, and next index `i+1`, then pop to backtrack.
- This ensures combinations use distinct increasing numbers and naturally avoids duplicates.

TC: `O(C(9, k) * k)` — choose `k` numbers from 1..9 and copy each valid combination.
SC: `O(k)` auxiliary space for recursion and `temp`, excluding output space.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<vector<int>>&ans, vector<int>&temp, int t, int n, int idx){
        if(n == 0){
            if(t == 0)  ans.push_back(temp);
            return;
        }
        for(int i = idx ; i < 10 ; i++){
            if(i > t) break;
            temp.push_back(i);
            solve(ans, temp, t-i, n-1, i+1);
            temp.pop_back();
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>>ans;
        vector<int>temp;
        solve(ans, temp, n, k, 1);

        return ans;
    }
};
```