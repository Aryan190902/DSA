**Question**: [Power of Four](https://leetcode.com/problems/power-of-four/)

**Approach**:
- Since powers of four are also power of 2, so we need to check if the 0's (binary value) in power of 2 are even or not.

TC: `O(1)`, Bitwise trick
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        int cnt = 0;
        while(n > 1){
            if(n&1) return false;
            n >>= 1;
            cnt++; 
        }

        return n > 0 && !(cnt&1);
    }
};
```