**Question**: [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

**Approach**:
    We can declare a prefix sum array, and just count the prefix sum once when we instatiate the NumArray object. So, the overall time complexity will be `O(n)`. The space complexity will also be `O(n)`.

    The brute force solution would be that whenever we call sumRange, we will calculate the sum from left to right, which would be `O(n)` for each call, resulting it in net `O(n<sup>2</sup>)`.

**Code**:

```cpp
class NumArray {
public:
    vector<int>pref{0};
    NumArray(vector<int>& nums) {
        for(int i : nums){
            pref.push_back(pref.back() + i);
        }
    }
    
    int sumRange(int left, int right) {
        // remove pref[right+1] - pref[left]
        return pref[right+1] - pref[left];
    }
};
```