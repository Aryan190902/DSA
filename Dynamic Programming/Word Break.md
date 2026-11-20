**Question**: [Word Break](https://leetcode.com/problems/word-break/)

**Approach 1: Top-down memoization**
- Define `solve(s, wordDict, idx)` as whether the suffix beginning at `idx` can be segmented into dictionary words.
- Try every dictionary word at the current position.
- If a word matches the beginning of the suffix, recursively check whether the remaining suffix can also be segmented.
- Memoize each index as `true` or `false`; `dp[idx]` answers whether `s[idx ... n-1]` is breakable.
- Return `true` immediately if the first successful segmentation reaches the end.

TC: `O(n * m * L)`, where $n$ is the string length, $m$ is the dictionary size, and $L$ is the maximum word length. The memoized recursion computes at most $n$ states, and each state checks $m$ words.

SC: `O(n)` for the memoization array and recursion stack.

**Code**:
```cpp
class Solution {
public:

    bool solve(string &s, vector<string>& w, int idx, vector<int>& dp) {
        // Reaching the end means every character has been matched by a word.
        if (idx == s.size()) return true;

        // Return a previously computed answer for this suffix.
        if (dp[idx] != -1) return dp[idx];

        // Try a word beginning at the current index.
        for (auto &str : w) {
            // compare(idx, str.size(), str) returns zero when the substrings match.
            if (s.compare(idx, str.size(), str) == 0) {

                // If the remaining suffix is breakable, the whole suffix is breakable.
                if (solve(s, w, idx + str.size(), dp)) {
                    return dp[idx] = true;
                }
            }
        }

        // No dictionary segmentation can cover this suffix.
        return dp[idx] = false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        // dp[idx] is -1 until the breakability of the suffix at idx is known.
        vector<int> dp(s.size() + 1, -1);
        return solve(s, wordDict, 0, dp);
    }
};
```

**Approach 2: Bottom-up dynamic programming**
- Define `dp[idx]` as whether the suffix `s[idx ... n-1]` can be segmented.
- Set `dp[n] = true`, because an empty suffix is valid once all characters have been consumed.
- Process indices from right to left so every later position needed for a word has already been calculated.
- At index `idx`, a word is valid if it fits within the string and matches `s.compare(idx, str.size(), str)`, and `dp[idx + str.size()]` is true.
- Once any matching word leads to a breakable suffix, set `dp[idx] = true` and stop checking other words.

TC: `O(n * m * L)`, where $n$ is the string length, $m$ is the dictionary size, and $L$ is the maximum word length.

SC: `O(n)` for the 1D DP table.

**Code**:
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        // dp[idx] indicates whether the suffix beginning at idx is breakable.
        vector<int> dp(s.size() + 1, 0);
        int n = s.size();

        // The empty suffix at index n is valid.
        dp[n] = true;

        // Solve larger suffixes first, moving toward the start of the string.
        for (int idx = n - 1; idx >= 0; idx--) {
            for (auto &str : wordDict) {
                int w = idx + str.size();

                // w must fit, the remaining suffix must be breakable, and str must match.
                if (w <= n && dp[w] && s.compare(idx, str.size(), str) == 0) {
                    dp[idx] = true;
                    break;
                }
            }
        }

        return dp[0];
    }
};
```