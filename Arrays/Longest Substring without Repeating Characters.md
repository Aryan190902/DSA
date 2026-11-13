**Question**: [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**Approach**:
- Use a sliding window `[start, i]` that always contains unique characters.
- Store each character's most recent index in a hash map.
- When `s[i]` has appeared before, move `start` past its previous index. Use `max` so the window's start never moves backward if that previous occurrence is outside the current window.
- Update the character's latest index, then calculate the current window length and update the maximum.

TC: `O(n)` average, where `n` is the length of the string.
SC: `O(min(n, |Σ|))` for the map, where `|Σ|` is the character-set size.

**Code**:
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ans = 0, start = 0;
        unordered_map<char, int> mp; // Most recent index of each character
        for (int i = 0; i < s.size(); i++) {
            if (mp.count(s[i]))
                // Keep the window start moving forward; never move it backward.
                start = max(start, mp[s[i]] + 1);

            mp[s[i]] = i; // Record this character's latest position
            ans = max(ans, i - start + 1); // Length of the current unique-character window
        }
        return ans;
    }
};
```