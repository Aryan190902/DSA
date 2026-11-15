**Question**: [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

**Approach**:
- Count how many times each value appears using a hash map.
- Use a min-heap of pairs `(frequency, value)`. Whenever the heap grows beyond `k`, discard the smallest-frequency element.
- After processing all numbers, the heap contains exactly the `k` elements with the highest frequencies (breaking ties by value only for heap ordering, not for the required result correctness).
- Extract the remaining values from the heap into the answer vector.

TC: `O(n + m log k)`, where `m` is the number of distinct values. Each distinct value is inserted once and the heap never exceeds `k` elements.
SC: `O(m + k)` for the frequency map and heap, with output storage excluded unless noted separately.

**Code**:
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        using Pair = pair<int, int>;
        // Min-heap ordered by frequency; if frequencies tie, value is used as second key.
        priority_queue<Pair, vector<Pair>, greater<Pair>> pq;
        unordered_map<int, int> mp;

        // Count frequency of each number.
        for (int x : nums) {
            mp[x]++;
        }

        // Keep only the k most frequent elements in the heap.
        for (const auto& [key, val] : mp) {
            pq.push({val, key});
            if (pq.size() > k) {
                pq.pop(); // Remove the smallest frequency currently in the heap.
            }
        }

        vector<int> ans;
        while (!pq.empty()) {
            ans.push_back(pq.top().second); // Value with current highest frequency among survivors.
            pq.pop();
        }

        return ans;
    }
};
```