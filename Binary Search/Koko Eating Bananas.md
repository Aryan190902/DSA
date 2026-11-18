**Question**: [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

**Approach**:
- Binary search for the minimum eating speed $k$ between $1$ and the largest pile.
- For a candidate speed, `solve` adds $\lceil pile / k \rceil$ hours for each pile.
- If Koko can finish within `h` hours, move the right boundary down; otherwise, move the left boundary up.
- When `l == r`, the boundaries meet at the minimum speed that can finish in time.

TC: $O(n \log(\max(piles)))$, checking each candidate against $n$ piles.
SC: $O(1)$ auxiliary space.

**Code**:
```cpp
class Solution {
public:
    long long solve(vector<int>& p, int k) {
        long long hrs = 0;
        for (int x : p) {
            hrs += (x + k - 1LL) / k;   // Integer ceil(x / k) without floating-point arithmetic
        }
        return hrs;
    }

    int minEatingSpeed(vector<int>& piles, int h) {
        // The largest pile is an upper bound because Koko can eat one pile per hour.
        int r = *max_element(piles.begin(), piles.end());
        int l = 1;

        while(l < r){
            int mid = l + (r-l)/2;
            if(solve(piles, mid) <= h){
                r = mid;          // This speed works, so try a slower speed.
            }
            else l = mid+1;      // This speed is too slow.
        }

        return l;
    }
};
```