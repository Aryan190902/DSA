**Question**: [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

**Approach**:
- Keep a stack of indices for days that have not yet found a warmer future day.
- As we scan temperatures from left to right, compare the current temperature with the day at the top of the stack.
- If the current day is warmer, it is the next warmer day for that earlier index. Record the distance, then pop the resolved index. Repeat in case the current day also resolves earlier days.
- Push the current index after resolving all days it is warmer than. Any indices left in the stack have no warmer day afterward, so their answer remains `0`.

TC: `O(n)`, since each index is pushed and popped at most once.
SC: `O(n)` for the stack and result vector.

**Code**:
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temp) {
        int n = temp.size();
        vector<int>ans(n, 0); // Default to 0 for days with no warmer future day
        stack<int>st; // Indices of days still waiting for a warmer temperature
        st.push(0);
        for(int i = 1 ; i < n ; i++){
            // The current day resolves earlier days that are cooler than it.
            while(!st.empty() && temp[st.top()] < temp[i]){
                ans[st.top()] = i - st.top(); // Number of days until this warmer temperature
                st.pop();
            }
            st.push(i); // This day may be resolved by a later warmer temperature
        }

        return ans;
    }
};
```