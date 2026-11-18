**Question**: [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

**Approach**:
- Find the rotation pivot, the smallest index, using binary search. Because the original array was sorted, `nums[mid] > nums[r]` means the pivot is to the right; otherwise, the pivot is at `mid` or to its left.
- Once the pivot is known, determine which of the two sorted segments contains the target.
- If `target` lies between `nums[pivot]` and `nums[r]`, search the right segment `[pivot, r]`; otherwise, search the left segment `[0, pivot]`.
- The helper performs an ordinary binary search and returns `-1` if the target is absent.

TC: `O(log n)` for the pivot search and the subsequent binary search.
SC: `O(1)` auxiliary space.

**Code**:
```cpp
class Solution {
public:
    int binarySearch(int l, int r, int t, vector<int>& v){
        while(l <= r){
            int mid = (l+r)/2;
            if(v[mid] == t) return mid;
            else if(v[mid] < t) l = mid+1;
            else r = mid-1;
        }
        return -1;
    }

    int search(vector<int>& nums, int target) {
        int n = nums.size();

        // Find the pivot: the starting index of the originally sorted array.
        int pivot = -1;
        int l = 0, r = n-1;
        while(l < r){
            int mid = (l+r)/2;
            if(nums[mid] > nums[r]){
                // The right side contains the pivot.
                l = mid+1;
            }else{
                // The right part is sorted, so the pivot is at or to the left of mid.
                r = mid;
            }
        }
        pivot = l;

        // Search the sorted segment that can contain the target.
        l = 0, r = n-1;
        int ans = 0;
        if(nums[pivot] <= target && target <= nums[r]){
            ans = binarySearch(pivot, r, target, nums);
        }else{
            ans = binarySearch(l, pivot, target, nums);
        }

        return ans;
    }
};
```