**Question**: [Letter Combinations of a Phone Number]()

**Approach**:

- Use a fixed mapping from digits to their letters (like on a phone keypad).
- Use backtracking: build a temporary string `temp` and at depth `idx = temp.size()` iterate over letters for `digits[idx]`.
- For each letter: append to `temp`, recurse to fill the next position, then pop back to backtrack.
- When `temp.size() == digits.size()` add `temp` to `ans`.
- Handle edge case: if input `digits` is empty, return an empty list.

TC: `O(4^d * d)` in worst case (each digit maps to up to 4 letters, `d = digits.length()`; building each string costs O(d)).
SC: `O(4^d * d)` for output plus `O(d)` recursion stack.

**Code**:
```cpp
class Solution {
public:

    void solve(vector<string>&mp, string &dig, string &temp, vector<string>&ans){
        if(temp.size() == dig.size()){
            ans.push_back(temp);
            return;
        }
        int idx = temp.size(); // the idx we need currently
        char x = dig[idx] - '0'; // the required digit, converted to int
        for(char c : mp[x]){
            temp += c;
            solve(mp, dig, temp, ans);
            temp.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        vector<string>mapp = { // value based mapping, mapp[2] = "abc"
            "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
        };
        string temp = "";
        vector<string>ans;
        solve(mapp, digits, temp, ans);
        return ans;
    }
};
```