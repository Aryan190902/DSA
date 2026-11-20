**Question**: [Coin Change](https://leetcode.com/problems/coin-change/)

**Approach 1: Top-down memoization**
- Define `solve(coins, amount, idx)` as the minimum number of coins needed to form the remaining target using coins from index `idx` onward.
- At each state, either stay on the current coin and reuse it after subtracting its value, or move to the next coin.
- Memoize each `(idx, remaining amount)` state because repeated target values occur through recursive reuse.
- Use `1e7` as an impossibility sentinel, since it is larger than any valid answer for the LeetCode constraints. If the final answer remains at least `1e7`, return `-1`.

TC: `O(n * amount)`, where `n = coins.size()` and each state $(idx, target)$ is computed once.

SC: `O(n * amount)` for the memoization table, plus `O(amount)` for the recursion stack in the worst case.

**Code**:
```cpp
class Solution {
public:

    int solve(vector<int>& c, int t, int idx, vector<vector<int>>& dp) {
        // Once all coins are considered, only a target of zero is solvable.
        if (idx >= c.size()) {
            if (t > 0) return 1e7; // No combination can form the remaining target.
            return 0;
        }

        // Return the already computed result for this state.
        if (dp[idx][t] != -1) return dp[idx][t];

        // Stay on the current coin and reuse it for the smaller target.
        int stay = 1e8;
        if (t >= c[idx])
            stay = 1 + solve(c, t - c[idx], idx, dp);

        // Move to the next coin without using the current coin again.
        int move = solve(c, t, idx + 1, dp);

        return dp[idx][t] = min(stay, move);
    }

    int coinChange(vector<int>& coins, int amount) {
        int c = INT_MAX;

        // dp[idx][t] stores the minimum coins for target t at coin index idx.
        vector<vector<int>> dp(coins.size(), vector<int>(amount + 1, -1));
        int ans = solve(coins, amount, 0, dp);

        return ans < 1e7 ? ans : -1;
    }
};
```

**Approach 2: 2D bottom-up dynamic programming**
- Use `dp[idx][t]` to represent the minimum coins needed for target `t` using coins from index `idx` onward.
- The base case `dp[n][0] = 0` means zero coins are needed for a target of zero once no coins remain.
- Fill the table backwards over coin indices. For each state, take the minimum of skipping the current coin and reusing it after subtracting its value.
- An unreachable target remains `INF`; return `-1` if `dp[0][amount]` is unreachable.

TC: `O(n * amount)`.

SC: `O(n * amount)` for the 2D DP table.

**Code**:
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        const int INF = 1e7;

        // dp[idx][t] represents the answer using coins from idx through n-1.
        vector<vector<int>> dp(coins.size() + 1, vector<int>(amount + 1, INF));
        int n = coins.size();

        // With no coins left, only target zero is possible.
        dp[n][0] = 0;

        // Fill states from the highest coin index toward the start.
        for (int idx = n - 1; idx >= 0; idx--) {
            for (int t = 0; t <= amount; t++) {

                // Skip the current coin and use the result for the next index.
                int move = dp[idx + 1][t];

                // Reuse the current coin after reducing the target by its value.
                int stay = (t >= coins[idx]) ? 1 + dp[idx][t - coins[idx]] : INF;

                dp[idx][t] = min(move, stay);
            }
        }

        // If the answer is still INF, no combination reaches amount.
        return dp[0][amount] >= INF ? -1 : dp[0][amount];
    }
};
```

**Approach 3: 1D bottom-up dynamic programming**
- Process each coin once in a 1D array, where `dp[t]` stores the minimum coins needed for target `t` using the coins already processed.
- The skip option is `dp[t]`, retained from the previous coin index.
- The reuse option is `1 + dp[t-coins[idx]]`, updated while processing the current coin. Iterating targets from low to high permits reuse of the current coin.
- `dp[0] = 0`, and any target that remains `INF` is impossible.

TC: `O(n * amount)`.

SC: `O(amount)` for the 1D DP table.

**Code**:
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        const int INF = 1e7;

        // dp[t] stores the minimum coins needed for target t.
        vector<int> dp(amount + 1, INF);
        int n = coins.size();

        // Zero coins are needed to form target zero.
        dp[0] = 0;

        // Process one coin denomination at each outer-loop iteration.
        for (int idx = n - 1; idx >= 0; idx--) {

            // Ascending targets allow dp[t - coins[idx]] to use the current coin again.
            for (int t = 0; t <= amount; t++) {

                // Keep the best answer using only previously processed coins.
                int move = dp[t];

                // Use the current coin and reuse it for a lower target.
                int stay = (t >= coins[idx]) ? 1 + dp[t - coins[idx]] : INF;

                dp[t] = min(move, stay);
            }
        }

        // A remaining INF means that this target cannot be formed.
        return dp[amount] >= INF ? -1 : dp[amount];
    }
};
```