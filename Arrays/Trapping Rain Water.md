**Question**: [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

**Approach 1: Prefix and suffix maximum arrays**
- At position `i`, trapped water is determined by the smaller boundary height on its left and right.
- Build `maxLeft[i]`, the maximum height from index `0` through `i`.
- Build `maxRight[i]`, the maximum height from index `i` through `n - 1`.
- For each position, add `min(maxLeft[i], maxRight[i]) - h[i]`, if positive.

TC: $O(n)$ for building both arrays and calculating the result.

SC: $O(n)$ for `maxLeft` and `maxRight`.

**Code**:
```cpp
class Solution {
public:
    int trap(vector<int>& h) {
        int n = h.size();

        // maxLeft[i] and maxRight[i] store the tallest boundary seen on each side.
        vector<int>maxLeft(n, 0), maxRight(n, 0);
        maxLeft[0] = h[0], maxRight[n-1] = h[n-1];

        for(int i = 1 ; i < n ; i++){
            maxLeft[i] = max(maxLeft[i-1], h[i]);
        }
        for(int i = n-2 ; i >= 0 ; i--) {
            maxRight[i] = max(maxRight[i+1], h[i]);
        }

        int ans = 0;
        for(int i = 0 ; i < n ; i++) {
            ans += max(min(maxLeft[i], maxRight[i]) - h[i], 0);
        }
        return ans;
    }
};
```

**Approach 2: Two pointers**
- Maintain left and right pointers and the maximum boundary height seen from each side.
- If `h[l] < h[r]`, the left maximum is the limiting boundary at `l`, because a boundary at least as tall as `h[r]` already exists on the right.
- Update `mL` when the left height is a new maximum; otherwise, add `mL - h[l]` to the answer.
- Similarly, process the right pointer when `h[l] >= h[r]`; the right maximum is then the limiting boundary.
- Each pointer moves inward once, so the full result is computed without auxiliary arrays.

TC: $O(n)$.

SC: $O(1)$ auxiliary space.

**Code**:
```cpp
class Solution {
public:
    int trap(vector<int>& h) {
        int n = h.size();
        int ans = 0;
        int l = 0, r = n-1;
        int mL = 0, mR = 0;

        while(l < r){
            if(h[l] < h[r]){
                if(h[l] >= mL)  mL = h[l]; // Height at l becomes a stronger left boundary.
                else    ans += mL - h[l]; // The left boundary can trap water above h[l].
                l++;
            }else{
                if(h[r] >= mR)  mR = h[r]; // Height at r becomes a stronger right boundary.
                else    ans += mR - h[r]; // The right boundary can trap water above h[r].
                r--;
            }
        }

        return ans;
    }
};
```