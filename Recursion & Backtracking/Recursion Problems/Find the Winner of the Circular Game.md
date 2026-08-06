**Question**: [Find the Winner of the Circular Game](https://leetcode.com/problems/find-the-winner-of-the-circular-game/)

**Approach**:
- Use the Josephus recurrence: if `J(i)` is the 0-indexed survivor position for `i` people, then `J(1) = 0` and `J(i) = (J(i-1) + k) % i`
- Build up the answer iteratively from a circle of size `1` to size `n`, instead of simulating the elimination process
- Each step shifts the previous survivor's position by `k` and wraps around using `%` to stay within the current circle size
- Finally, convert the 0-indexed result to 1-indexed by adding `1`

TC: `O(n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int findTheWinner(int n, int k) {
        int ans = 0; // J(1) = 0, base case
        for (int i = 2; i <= n; i++) {
            ans = (ans + k) % i; // J(i) = (J(i-1) + k) % i
        }
        return ans + 1;
    }
};
```