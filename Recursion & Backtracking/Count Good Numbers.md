**Question**: [Count Good Numbers](https://leetcode.com/problems/count-good-numbers/)

**Approach**:
- Create a function to calculate power.
- Since there are only four 1 digit prime numbers, then $4^{n/2}$ will be the number of prime positions and $5^{(n+1)/2}$ will be the number of even positions.

TC: `O(logn)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    long long findPower(long long x, long long n){
        long long m = n;
        long long ans = 1;
        while(m > 0){
            if(m&1) ans = (ans*x)%MOD;
            x = (x*x)%MOD;
            m /= 2;
        }
        return ans;
    }

    int countGoodNumbers(long long n) {
        // 4 ^ float(n/2) * 5 ^ ceil(n/2) => 20 ^ float(n/2) * 5 if n == odd
        if(n == 1)  return 5;
        long long pow5 = findPower(5, (n+1)/2);
        long long pow4 = findPower(4, n/2);
        long long ans = (pow5*pow4)%MOD;
        return (int)ans;
    }
};
```