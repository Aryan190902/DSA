**Question**: [Minimum Non-Zero Product of the Array Elements]()

**Approach**: 
Check this out: [Solution](https://leetcode.com/problems/minimum-non-zero-product-of-the-array-elements/solutions/1421223/c-simple-and-easy-solution-detailed-expl-d1vu/)

**Code**:
```cpp
#define ll long long
class Solution {
public:
    const ll MOD = 1e9 + 7;

    ll modPow(ll base, ll exp) {
        ll res = 1;
        base %= MOD;
        while (exp > 0) {
            if (exp & 1LL) res = res * base % MOD;
            base = base * base % MOD;
            exp >>= 1LL;
        }
        return res;
    }

    int minNonZeroProduct(int p) {
        ll maxVal = (1LL << p) - 1;
        ll pairs = (1LL << (p - 1)) - 1;
        ll ans = (maxVal % MOD) * modPow(maxVal - 1, pairs) % MOD;
        return (int)ans;
    }
};
```