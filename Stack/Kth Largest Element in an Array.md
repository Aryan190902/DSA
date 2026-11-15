**Question**: [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

**Approach**:
- Maintain a min-heap of the current `k` largest numbers seen so far.
- For each number in the array, insert it into the heap.
- If the heap grows larger than `k`, remove the smallest element; this keeps only the `k` largest values.
- After processing the full array, the heap’s top is the smallest value among the `k` largest numbers, which is exactly the `k`th largest element.

TC: `O(n log k)`, because each insertion may trigger a heap pop and each heap operation costs `O(log k)`.
SC: `O(k)` for the heap, excluding the input array itself.

**Code**:
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        // Min-heap holding the current k largest elements.
        priority_queue<int, vector<int>, greater<int>> pq;

        for (int x : nums) {
            pq.push(x);

            // Keep only the k largest values seen so far.
            if ((int)pq.size() > k) {
                pq.pop(); // Drop the smallest value currently in the heap.
            }
        }

        // The top of the min-heap is the kth largest element.
        return pq.top();
    }
};
```