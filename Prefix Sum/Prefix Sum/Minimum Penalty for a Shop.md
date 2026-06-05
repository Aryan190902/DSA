**Question**: [Minimum Penalty for a Shop](https://leetcode.com/problems/minimum-penalty-for-a-shop/)

**Approach**:
We track how the penalty changes as the closing time moves from left to right.

- Closing one hour later:
    - If the customer at that hour is `'Y'`, the penalty decreases by 1 (we serve that customer instead of missing them).
    - If the customer is `'N'`, the penalty increases by 1 (the shop stays open unnecessarily).
- Maintain a running prefix sum (`pref`) representing the net change in penalty from the initial closing time of 0.
- Whenever `pref` reaches a new minimum value, it means the current closing time yields a lower penalty than any previous choice, so we update `besttime` to `i + 1`.
- The earliest time corresponding to the minimum penalty is returned.

Time Complexity: `O(n)`
Space Complexity: `O(1)`

**Code**:
```cpp
class Solution {
public:
    int bestClosingTime(string customers) {
        int besttime = 0;
        int minPen = 0;
        int pref = 0;
        for(int i = 0 ; i < customers.length() ; i++){
            pref += customers[i] == 'Y' ? -1 : 1;
            if(pref < minPen){
                besttime = i+1;
                minPen = pref;
            }
        }

        return besttime;
        
    }
};
```