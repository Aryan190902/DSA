**Question**: [Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

**Approach**:
- Use recursion with memoization on indices `(i, j)` for string `s` and pattern `p`.
- If both indices are exhausted, the match succeeds; if only the pattern remains, it must be all `*` characters.
- If current characters match or pattern has `?`, move both indices left and continue.
- If the current pattern character is `*`, either treat it as matching no character (`i, j-1`) or match one character and stay on `*` (`i-1, j`).
- Store results in `dp` so each state is computed once and repeated work is avoided.

TC: `O(n*m)`
SC: `O(n*m)`

**Code**:
```cpp
class Solution {
public:
    bool solve(pair<int, int>idx, string &s, string &p, vector<vector<int>>&dp){
        int i = idx.first, j = idx.second;
        int n = s.length(), m = p.length();

        if(i < 0 && j < 0)    return true;
        if(j < 0)  return false;
        if(i < 0 && j >= 0) return checkStars(p, j);
        if(dp[i][j] != -1) return dp[i][j];

        bool check = (s[i] == p[j] || p[j] == '?');
        if(check)   return dp[i][j] = solve({i-1, j-1}, s, p, dp);

        if(p[j] == '*'){
            bool empty = solve({i, j-1}, s, p, dp);
            bool oneOrMore = solve({i-1, j}, s, p, dp);

            return dp[i][j] = empty || oneOrMore;
        }

        return dp[i][j] = false;
    }

    bool checkStars(string &p, int idx){
        for(int i = 0 ; i <= idx ; i++){
            if(p[i] != '*') return false;
        }
        return true;
    }

    bool isMatch(string s, string p) {
        int n = s.length(), m = p.length();
        vector<vector<int>>dp(n, vector<int>(m, -1));
        bool ans = solve({n-1,m-1}, s, p, dp);
        return ans;
    }
};
```