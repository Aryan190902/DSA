**Question**: [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

**Approach**:
- Insert all numbers into an unordered set so membership checks take $O(1)$ average time and duplicates are removed.
- Only start counting from a number `i` when `i - 1` is not in the set. This ensures each consecutive sequence is scanned once, from its smallest value.
- Walk forward through the set to find the sequence length and update the maximum.

TC: `O(n)` average, since each number is visited a constant number of times.
SC: `O(n)` for the unordered set.

**Code**:
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st(nums.begin(), nums.end());
        int ans = 0;
        for (int i : st) {
            if (!st.count(i - 1)) { // If previous number not available, this is the first element of the sequence.
                int x = i + 1;
                while (st.count(x)) x++; // Find the sequence's last element
                ans = max(ans, x - i);
            }
        }
        return ans;
    }
};
```