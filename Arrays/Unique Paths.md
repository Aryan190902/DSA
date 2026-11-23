**Question**: [Unique Paths](https://leetcode.com/problems/unique-paths/)

**Approach**:
- Every path from the top-left to the bottom-right uses exactly $m - 1$ downward moves and $n - 1$ rightward moves.
- Thus, the number of distinct paths is the number of ways to choose the positions of either set of moves among $m + n - 2$ total moves:

$$
\binom{m+n-2}{m-1}=\binom{m+n-2}{n-1}
$$

- Let $k$ be the smaller of the two move counts. This computes $\binom{m+n-2}{k}$ while keeping the number of loop iterations minimal.
- Starting from $\binom{t}{0}=1$, the loop incrementally computes $\binom{t}{i}=\binom{t}{i-1}\times\frac{t-i+1}{i}$. The expression `totalSteps + i - small` starts at $t-k+1$ and ends at $t$, producing $\binom{t}{k}$.

TC: $O(m + n)$, since the loop runs at most $\min(m-1, n-1)$ times.

SC: $O(1)$ auxiliary space.

**Code**:
```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        // Every path requires m-1 down moves and n-1 right moves.
        int totalSteps = m+n-2;
        int rightSteps = n-1, downSteps = m-1;

        // Calculate C(totalSteps, smaller of rightSteps and downSteps).
        long long ans = 1;
        int small = min(rightSteps, downSteps);
        for(int i = 1 ; i <= small ; i++){
            ans = ans*(totalSteps + i - small)/i;
        }
        return ans;
    }
};
```