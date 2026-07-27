**Question**: [Predict the Winner](https://leetcode.com/problems/predict-the-winner)

**Approach**:
- Use recursion to simulate all the possible scenerios, since the contraints are `1 <= nums.length <= 20`.
- Player 1's turn, we should use `l || r`, since if in either case he wins, then return true.
- Player 2's turn, we need to find if in any case player 1 can win or not, so `l && r`, meaning player 1's win doesn't get affected by player 2's turn.

TC: $O(2^n)$
SC: `O(n)`, that's the maximum recursion depth.

**Code**:

```cpp
class Solution {
public:
    bool solve(vector<int>&v, pair<int, int>p, pair<int, int>score, int ply, int minVal){
        if(score.first >= minVal)   return true;
        if(p.first > p.second)  return false;
        bool l, r;
        if(ply == 1){
            // pick left
            l = solve(v, {p.first+1, p.second}, {score.first+v[p.first], score.second}, 2, minVal);
            // pick right
            r = solve(v, {p.first, p.second-1}, {score.first + v[p.second], score.second}, 2, minVal);
            return l || r;
        }
        // For player 2
        l = solve(v, {p.first+1, p.second}, {score.first, score.second+v[p.first]}, 1, minVal);
        // pick right
        r = solve(v, {p.first, p.second-1}, {score.first, score.second+ v[p.second]}, 1, minVal);
        return (l && r);
    }

    bool predictTheWinner(vector<int>& nums) {
        int n = nums.size();
        pair<int, int>bounds{0, n-1};
        int totalScore = 0;
        for(int i : nums){
            totalScore += i;
        }
        int minScoreToWin = ceil((float)totalScore / 2.0);
        int p1Score = 0, p2Score = 0;
        bool ans = solve(nums, bounds, {p1Score, p2Score}, 1, minScoreToWin);
        return ans;
    }
};
```

#### Optimized Solution:
- Use Minimax/Recursion with Memoization.

TC: $O(N^2)$
DSC: $O(N^2)$

**Code**:
```cpp
class Solution {
public:

    int getScore(vector<int>&v, int l, int r, vector<vector<int>>&dp){
        if(l > r)   return 0;
        if(l == r)  return v[l];
        if(dp[l][r] != -1)    return dp[l][r];
        int currScore = max(
            v[l] + min( // P1 chose left
                getScore(v, l+2, r, dp), 
                getScore(v, l+1, r-1, dp)
            ),
            v[r] + min( // P1 chose right
                getScore(v, l, r-2, dp),
                getScore(v, l+1, r-1, dp)
            )
        );
        return dp[l][r] = currScore;
    }

    bool predictTheWinner(vector<int>& nums) {
        int total = 0;
        for(int i : nums){
            total += i;
        }
        int n = nums.size();
        int scoreToWin = ceil((float)total/2.0);
        vector<vector<int>>dp(n, vector<int>(n, -1));

        int p1Score = getScore(nums, 0, n-1, dp);
        return p1Score >= total - p1Score;
    }
};
```