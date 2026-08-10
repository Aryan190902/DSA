**Question**: [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)

**Approach**:

- Use DFS/backtracking over the string positions; `idx` represents the current position.
- If `s[idx]` is a digit, copy it to `temp` and recurse to `idx+1` (only one branch).
- If it's a letter, branch twice: push lowercase, recurse; pop; push uppercase, recurse; pop.
- When `idx == s.size()` add `temp` to `ans`.
- This generates all permutations of letter cases while preserving digits.

TC: `O(2^L * N)` where `L` is number of letters and `N` is string length (building each result costs O(N)).
SC: `O(2^L * N)` for output plus `O(N)` recursion stack.

**Code**:
```cpp
class Solution {
public:

    void solve(string &s, vector<string>&ans, string &temp, int idx){
        if(idx == s.size()){
            ans.push_back(temp);
            return;
        }
        
        if(isdigit(s[idx])){
            temp.push_back(s[idx]);
            solve(s, ans, temp, idx+1);
            temp.pop_back();
            return;
        }
        // lowercase
        char c = tolower(s[idx]);
        temp.push_back(c);
        solve(s, ans, temp, idx+1);
        temp.pop_back();

        // uppercase
        c = toupper(s[idx]);
        temp.push_back(c);
        solve(s, ans, temp, idx+1);
        temp.pop_back();
    }

    vector<string> letterCasePermutation(string s) {
        vector<string>ans;
        string temp = "";
        solve(s, ans, temp, 0);
        return ans;
    }
};
```