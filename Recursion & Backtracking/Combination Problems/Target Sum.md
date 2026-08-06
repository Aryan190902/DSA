**Question**: [Target Sum](https://leetcode.com/problems/target-sum/)

**Approach**:
- *Recursion*: start by trying two choices at each number: add it to the positive subset or subtract it from the negative subset. The state can be defined as `ways(i, remaining)` meaning the number of ways to form the required difference using numbers from index `i` onward.
- *Memoization*: the recursion has overlapping subproblems, so store results for each `(i, remaining)` in a DP table to avoid recomputing the same states.
- *Tabular format*: transform the problem into a subset-sum one. If `S(P) - S(N) = target` and `S(P) + S(N) = total`, then `S(P) = (target + total) / 2`. Now the task becomes counting subsets whose sum is `newTar`.
- *Space optimization*: instead of a 2D DP table, use a 1D array `dp` where `dp[j]` stores the number of ways to make sum `j` using processed numbers. Traverse `j` backward so each number is used at most once.

**TC:** $O(n \times S)$, where $S = \frac{total + target}{2}$

**SC:** $O(S)$

**Code**:
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int n = nums.size();
        /*
            Let's split the array into subset of pos(P), and neg(N)
            S(P) - S(N) = target
            S(P) + S(N) = total
            S(P) = (target + total)/2

            facts-> 
            1. abs(target) > total => no solution
            2. (target + total)%2 != 0 => no solution
        */

        int tot = accumulate(nums.begin(), nums.end(), 0);
        if(abs(target) > tot || (tot + target)%2 != 0)  return 0;
        int newTar = (tot + target)/2;

        vector<int>dp(newTar+1, 0);
        dp[0] = 1; // s(p) is empty set
        
        for(int num : nums){
            for(int j = newTar ; j >= num ; j--){
                dp[j] += dp[j-num];
            }
        }

        return dp[newTar];
    }
};
```