**Question**: [Find Good Days to Rob the Bank](https://leetcode.com/problems/find-good-days-to-rob-the-bank/)

**Approach**:
- Precompute a `noninc` array where `noninc[i]` stores the number of consecutive non-increasing days ending at index `i`.
- Precompute a `nondec` array where `nondec[i]` stores the number of consecutive non-decreasing days starting at index `i`.
- Build `noninc` by traversing from left to right and nondec by traversing from right to left.
- For each day `i`, check whether `noninc[i]` >= time and `nondec[i]` >= time.
- If both conditions are satisfied, add i to the result as it has at least time non-increasing days before it and at least time non-decreasing days after it.

```cpp
class Solution {
public:

    vector<int> goodDaysToRobBank(vector<int>& security, int time) {
        int n = security.size();
        vector<int>nondec(n, 0), noninc(n, 0); // precompute all nondec, and noninc
        vector<int>ans;
        for(int i = 1 ; i < n ; i++){
            if(security[i] <= security[i-1]){
                noninc[i] = noninc[i-1] + 1;
            }
            if(security[n-i-1] <= security[n-i]){
                nondec[n-i-1] = nondec[n-i] + 1;
            }
        }
        for(int i = 0 ; i < n ; i++){
            if(noninc[i] >= time && nondec[i] >= time)  ans.push_back(i);
        }

        return ans;
    }
};
```