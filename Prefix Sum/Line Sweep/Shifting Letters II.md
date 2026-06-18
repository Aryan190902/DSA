**Question**: [Shifting Letters II](https://leetcode.com/problems/shifting-letters-ii/)

**Approach**: 
Using DAT (Difference Array Technique), we can compile all the changes, and just add the changes to the string for final result.
Remember to handle the negative or shift > 26.

TC: `O(n)`
SC: `O(n)`

**Code**:
```cpp
class Solution {
public:
    string shiftingLetters(string s, vector<vector<int>>& shifts) {
        // Use the difference array technique to calc all the changes first
        int n = s.length();
        vector<int>change(n+1, 0);
        for(vector<int>&v : shifts){
            int st = v[0], en = v[1], dir = v[2];
            if(dir){
                change[st]++;
                change[en+1]--;
            }
            else{
                change[st]--;
                change[en+1]++;
            }
        }
        for(int i = 0 ; i < n ; i++){
            change[i] += i > 0 ? change[i-1] : 0;
        }

        // final string after the changes
        for(int i = 0 ; i < n ; i++){
            int currAlpha = s[i] - 'a';
            // To handle neg values or values above 26
            int netShift = change[i]%26;
            int resAlpha = (currAlpha + netShift+ 26)%26;
            s[i] = 'a' + resAlpha;
        }

        return s;
    }
};
```