**Question**: [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

**Approach**:
- Just do as said

TC: `O(n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int fib(int n) {
        if(n == 0 || n == 1)    return n;
        int x = 0, y = 1;
        for(int i = 0 ; i <= n-2 ; i++){
            int temp = x + y;
            x = y;
            y = temp;
        }
        return y;
    }
};
```