**Question**: [Power of Three](https://leetcode.com/problems/power-of-three/)

**Approach**:
- Find the largest number less than INT_MAX that is a power of 3, and find the modulo

TC: `O(1)`, Just one modulo
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        // Without loops, we can just get the maximum possible value of 3^x present in INT
        // Then just check if 3^x % n == 0
        int max3 = 1162261467;
        return n > 0 && max3 % n == 0;

        // else just use loop and find the solution
    }
};
```