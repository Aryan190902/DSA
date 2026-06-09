**Question**: [Removing Minimum Number of Magic Beans](https://leetcode.com/problems/removing-minimum-number-of-magic-beans/)

**Approach**:
We can solve this question simply by calculating total number of beans, and checking if all beans are equal to $i^{th}$ bean, then how many beans do we need to remove. 

- First we sort in descending order (can also be done in ascending).
- Find saved beans (if all beans are equal, then for `j <= i, beans[j] >= beans[i]`, we can add them, and the rest of them will be `0`).
- Find the minimum value.

TC: `O(nlogn)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    long long minimumRemoval(vector<int>& beans) {
        sort(beans.begin(), beans.end(), greater<int>()); // sort in descending order
        long long ans = LLONG_MAX;
        long long total = 0;
        for(int bean : beans){
            total += bean;
        }
        int n = beans.size();
        for(int i = 0 ; i < n ; i++){
            long long saved = (long long)(beans[i]) * (long long)(i+1);
            long long removed = total - saved;
            ans = min(ans, removed);
        }

        return ans;
    }
};
```