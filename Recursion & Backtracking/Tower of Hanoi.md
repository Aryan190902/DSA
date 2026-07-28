**Question**: [Tower of Hanoi](https://www.geeksforgeeks.org/problems/tower-of-hanoi-1587115621/1)

**Approach**: 
Reached the logic through this:
- If we want to move the last disk ($n^{th}$), we need to first move all the $(n-1)$ disks above it
- Then we will move the last disk
- Then we will again move all the $n-1$ disks, that means $f(n) = f(n-1) + f(n-1) + 1 => f(n) = 2*f(n-1) + 1$
- For iterative solution, since we just need to keep the $n-1$ value in check, use a variable.

TC: `O(n)` For optimized solution
SC: `O(1)`

**Code**:
```cpp
class Solution {
  public:
    int towerOfHanoi(int n, int from, int to, int aux) {
        // Recursive solution
        // if(n == 0 || n == 1) return n;
        // return 2*towerOfHanoi(n-1, from, to, aux) + 1;

        // Just a for loop
        if(n == 0 || n == 1)    return n;
        int ans = 1;
        
        for(int i = 2 ; i <= n ; i++){
            ans = 2*ans + 1;
        }
        return ans;
    }
};
```