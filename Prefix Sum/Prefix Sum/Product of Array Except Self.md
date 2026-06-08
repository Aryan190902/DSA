**Question**: [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

**Approach**:
We calculate the prefix and suffix arrays, and then just multiply each index accordingly.

TC: `O(n)`
SC: `O(n)`
**Code**:
Using memory
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int>ans;
        vector<int>pref{1};
        vector<int>suff(n+1, 1);

        //prefix
        for(int i = 1 ; i < n ; i++){
            pref.push_back(pref[i-1]*nums[i-1]);
        }

        //suffix
        for(int i = n-1 ; i >= 0 ; i--){
            suff[i] = suff[i+1]*nums[i];
        }

        // answer should be: pref[i]*suff[i+1]
        for(int i = 0 ; i < n ; i++){
            ans.push_back(pref[i]*suff[i+1]);
        }

        return ans;
    }
};
```
#### Optimized Solution
**Approach**:
We don't really need pref, and suff. We will store the product directly into the ans array. 
We start with `curr = 1`, which can act like the previous `pref` value and loop through `nums`.
Then again start with `curr = 1`, and now it acts like the previous `suff` value.
TC: `O(n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int>ans(n, 1);
        int curr = 1;

        for(int i = 0 ; i < n ; i++){
            ans[i] *= curr;
            curr *= nums[i];
        }
        curr = 1;
        for(int i = n-1 ; i >= 0 ; i--){
            ans[i] *= curr;
            curr *= nums[i];
        }

        return ans;
    }
};
```