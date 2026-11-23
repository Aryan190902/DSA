**Question**: [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

**Approach 1: Top-down memoization**
- Define `solve(s1, s2, {i, j})` as the length of the longest common subsequence between suffixes `s1[i...]` and `s2[j...]`.
- If the characters at `i` and `j` match, they can both be included and the remaining length is `1 + solve(i + 1, j + 1)`.
- If they do not match, skip exactly one character in each of the two possible branches, taking the maximum of `solve(i + 1, j)` and `solve(i, j + 1)`.
- Memoize each `(i, j)` state so every distinct suffix pair is computed once.

TC: $O(n \times m)$, where $n$ and $m$ are the lengths of `text1` and `text2`.

SC: $O(n \times m)$ for the memoization table, plus $O(n + m)$ for the recursive call stack in the worst case.

**Code**:
```cpp
class Solution {
public:

    int solve(string &s1, string &s2, pair<int, int>p, vector<vector<int>>&dp){
        int i = p.first, j = p.second;

        // Reaching the end of either string means no more characters can match.
        if(i >= s1.size() || j >= s2.size())    return 0;

        // Reuse the answer for this pair of suffix positions.
        if(dp[i][j] != -1)  return dp[i][j];

        if(s1[i] == s2[j])
            return dp[i][j] = 1 + solve(s1, s2, {i+1, j+1}, dp);

        // Try skipping a character from either string and take the longer result.
        return dp[i][j] = max(solve(s1, s2, {i+1, j}, dp), solve(s1, s2, {i, j+1}, dp));
    }

    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>>dp(text1.size()+1, vector<int>(text2.size()+1, -1));
        int ans = solve(text1, text2, {0,0}, dp);
        return ans;
    }
};
```

**Approach 2: 2D bottom-up dynamic programming**
- Use `dp[i][j]` for the LCS length of the suffixes `text1[i...]` and `text2[j...]`.
- Process the table from the end of both strings toward the start, so `dp[i + 1][j + 1]`, `dp[i + 1][j]`, and `dp[i][j + 1]` are already known.
- If the current characters match, take `1 + dp[i + 1][j + 1]`.
- Otherwise, take the maximum of skipping `text1[i]` or skipping `text2[j]`.
- The extra final row and column remain zero, representing an empty suffix.

TC: $O(n \times m)$.

SC: $O(n \times m)$ for the 2D DP table.

**Code**:
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size(), m = text2.size();

        // dp[i][j] stores the LCS length of text1[i...] and text2[j...].
        vector<vector<int>>dp(n+1, vector<int>(m+1, 0));

        // Solve every pair from the end toward the beginning.
        for(int i = n-1 ; i >= 0 ; i--) {
            for(int j = m-1 ; j >= 0 ; j--) {
                if(text1[i] == text2[j]) {
                    dp[i][j] = 1 + dp[i+1][j+1];
                    continue;
                }

                // Skip one character from either string and keep the better result.
                dp[i][j] = max(dp[i+1][j], dp[i][j+1]);
            }
        }

        return dp[0][0];
    }
};
```

**Approach 3: 1D bottom-up dynamic programming**
- Keep only `curr`, the answer row for the next `i`, and `nxt`, the answer row currently being built.
- Processing `j` from right to left allows `nxt[j + 1]` to represent the diagonal and `curr[j + 1]` to represent the next row at the needed position.
- For matching characters, use `1 + curr[j + 1]`; otherwise, take `max(curr[j], nxt[j + 1])`.
- Swap the two rows after each outer-loop iteration, reducing the DP memory from a full table to two rows.

TC: $O(n \times m)$.

SC: $O(m)$ for the two 1D DP rows.

**Code**:
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size(), m = text2.size();

        // curr is the previously computed row; nxt is the row being built.
        vector<int>curr(m+1, 0), nxt(m+1, 0);

        for(int i = n-1 ; i >= 0 ; i--) {
            for(int j = m-1 ; j >= 0 ; j--) {
                if(text1[i] == text2[j]) {
                    nxt[j] = 1 + curr[j+1];
                    continue;
                }

                // curr[j] skips text1[i]; nxt[j+1] skips text2[j].
                nxt[j] = max(curr[j], nxt[j+1]);
            }
            swap(nxt, curr);
        }

        return curr[0];
    }
};
```