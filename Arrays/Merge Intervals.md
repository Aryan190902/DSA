**Question**: [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

**Approach**:
- Sort the intervals by starting value and then by ending value, so the next interval can only overlap or extend the interval currently being merged.
- Keep `temp` as the current merged interval. If its end is less than the next interval's start, the intervals are disjoint, so store `temp` and begin a new one.
- Otherwise, the intervals overlap (or touch) and `temp[1]` is updated to the maximum end value.
- The final merged interval is added after the loop; sorting mutates the input and $n$ is the number of intervals.

TC: $O(n \log n)$ for sorting, with $O(n)$ for scanning and storing the result.

SC: $O(n)$ auxiliary space for the output merged intervals; sorting is in place.

**Code**:
```cpp
class Solution {
public:

    static bool cmp(const vector<int>&a ,const vector<int>&b){
        if(a[0] == b[0])    return a[1] < b[1];
        return a[0] < b[0];
    }

    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        vector<vector<int>>ans;
        int n = intervals.size();
        vector<int>temp(2);
        temp[0] = intervals[0][0], temp[1] = intervals[0][1];
        // Sorting ensures every later interval starts at or beyond the current interval's start.
        for(int i = 1 ; i < n ; i++){
            const vector<int>&b = intervals[i];
            if(temp[1] < b[0]){ // Disjoint intervals: finalize the current merge.
                ans.push_back(temp);
                temp = b;
            }
            else{ // Overlapping or adjacent intervals: keep the farthest end.
                temp[1] = max(temp[1], b[1]);
            }
        }
        ans.push_back(temp);
        return ans;
    }
};
```