**Question**: [Pow(x,n)](https://leetcode.com/problems/powx-n/)

**Approach**:
- Check if x == 1 or n == 0, result will always be 1
- Use Binary Exponentiation (also known as Exponentiation by Squaring) to achieve O(log n) time complexity.
- If n is negative, get the abs value, and divide the result by 1.

TC: `O(logn)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    double myPow(double x, int n) {
        bool neg = false;
        long int m = n;
        if(m < 0){
            m = -m;
            neg = true;
        }
        if(m == 0 || x == 1)  return 1;
        double res = 1;
        while(m > 0){
            if(m&1){
                res *= x;
                m--;
            }else{
                x *= x;
                m /= 2;
            }
        }
        return neg ? (double)1/res : res;
    }
};
```