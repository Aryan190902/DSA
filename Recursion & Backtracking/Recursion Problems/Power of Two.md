**Question**: [Power of Two](https://leetcode.com/problems/power-of-two/)

**Approach**:
- Use binary approach to solve this.

TC: `O(1)`, Bitwise trick: `n & (n - 1)` removes the lowest set bit. A power of two has exactly one bit set.
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    bool isPowerOfTwo(int n) {
        // If n is power of 2, it's binary will be 1000... something
        // And it's previous number's binary will be 111... something
        /*
            For example n = 8. binary = 1000, n-1 = 7, binary = 111
            8&7 = 1000 & 0111 = 0000 = 0 = false
        */
        return (n>0) && !(n&(n-1));
    }
};
```