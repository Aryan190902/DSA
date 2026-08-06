**Question**: [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

**Approach**:
- Use DP on indices $(i, j)$ of the string and pattern to decide whether the suffixes match.
- If both indices are out of bounds, the match is successful; if the pattern is exhausted first, it fails.
- If the current pattern character is `.` or matches the current character in the string, we can move forward on both sides.
- If the next pattern character is `*`, we have two choices: `solve(i+1, j)` means we use the `*` to match one more occurrence of the current character, while `solve(i, j+2)` means we skip the current pattern character and its `*` entirely, treating `c*` as an empty match.
- Memoize states so each $(i, j)$ is solved only once and the recursion stays efficient.

TC: `O(n*m)`
SC: `O(n*m)`

**Code**:
```cpp
class Solution {
public:
    map<pair<int, int>, bool> dp;
    bool solve(pair<int, int>idx, string &s, string &p){
        int m = s.length(), n = p.length();
        int i = idx.first, j = idx.second;

        // If both i, j are out of bounds, return true
        if(i >= m && j >= n)    return true;
        if(j >= n)  return false; // else return false

        if(dp.contains({i, j})) return dp[{i, j}];        

        // Check if current chars match or not
        bool check = (i < m) && (s[i] == p[j] || p[j] == '.');

        /* 
            if next char is *, do this:
            - check if current are same
            - Two cases:
                1. Consider * as the current character
                2. Ignore the current and * character, since we can consider c* as empty string as well 
        */
        if((j+1) < n && p[j+1] == '*'){
            return dp[{i, j}] = check && solve({i+1, j}, s, p) || solve({i, j+2}, s, p);
        }

        // If next character isn't *, just check the current char
        if(check)   return dp[{i, j}] = solve({i+1, j+1}, s, p);
        
        return dp[{i, j}] = false;
    }

    bool isMatch(string s, string p) {
        bool ans = solve({0,0}, s, p);
        return ans;
    }
};
```