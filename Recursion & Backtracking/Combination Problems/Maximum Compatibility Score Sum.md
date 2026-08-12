**Question**: [Maximum Compatibility Score Sum](https://leetcode.com/problems/maximum-compatibility-score-sum/)

**Approach**:

- Precompute compatibility scores between each student and each mentor using `getScore`.
- Use a bitmask DP over mentor assignments: each mask `j` represents which mentors are already assigned.
- Iterate students in order, and for each mask, try assigning any unused mentor `k` to the current student.
- Update `dp[j] = max(dp[j], dp[j | (1<<k)] + scores[i][k])` so smaller masks build from larger masks.
- The answer is `dp[0]`, representing no mentors assigned yet and all students remaining.

TC: `O(m^2 * n + m * 2^m * m)` => `O(m^2 * n + m^2 * 2^m)`.
SC: `O(2^m + m^2)`.

**Code**:
```cpp
class Solution {
public:

    int getScore(vector<int>&st, vector<int>&mn){
        int ans = 0;
        for(int i = 0 ; i < st.size() ; i++){
            ans += (st[i] == mn[i]);
        }
        return ans;
    }

    int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) {
        int m = students.size(), n = students[0].size();
        vector<vector<int>>scores(m, vector<int>(m, 0));

        // O(n*m^2)
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < m ; j++){
                scores[i][j] = getScore(students[i], mentors[j]);
            }
        }

        // Creating bitmask number of values
        // Each bitmask represents the mentors already taken

        vector<int>dp((1<<m), 0);
        for(int i = 0 ; i < m ; i++){ // each student
            for(int j = 0 ; j < (1<<m) ; j++){ // all taken cases
                for(int k = m-1 ; k > -1 ; k--){ // each teacher
                    if(!(j & (1 << k))){ 
                        // j & 1<<k tells if teacher is taken or not
                        // Since j will be something like 0010, and 1<<k will be 0100
                        // the result will tell that teacher isn't taken
                        dp[j] = max(dp[j], dp[j | (1 << k)] + scores[i][k]);
                        // Here j | (1 << k) means that teacher is taken
                    }
                }
            }
        }

        return dp[0];

    }
};
```