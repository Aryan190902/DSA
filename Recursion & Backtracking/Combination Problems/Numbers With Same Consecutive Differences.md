**Question**: [Numbers With Same Consecutive Differences](https://leetcode.com/problems/numbers-with-same-consecutive-differences/)

**Approach**:
- Start the number with every digit from 1 to 9 to avoid leading zeroes.
- Recursively append `last - k` and `last + k` when they are valid digits.
- When the number reaches `n` digits, add it to the answer.
- For `k = 0`, only one branch is explored because both choices are identical.

TC: O(9 * 2^n)
SC: O(n) recursion stack, excluding the output

**Code**:
```cpp
class Solution {
public:
    vector<int>ans;

    void solve(int n, int k, int tmp){
        if(n == 1){
            ans.push_back(tmp);
            return;
        }
        int last = tmp%10;

        if(k != 0 && last-k >= 0){
            int y = tmp*10 + last-k;
            solve(n-1, k, y);
        }
        if(last+k < 10){
            int y = tmp*10 + last+k;
            solve(n-1, k, y);
        }
    }

    vector<int> numsSameConsecDiff(int n, int k) {
        for(int i = 1 ; i < 10 ; i++){
            solve(n, k, i);
        }
        return ans;
    }
};
```