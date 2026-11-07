**Question**: [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

**Approach**:
- Use a hash map whose key represents the frequency of each of the 26 lowercase English letters, and whose value stores the strings with that frequency pattern.
- Build the frequency key for each string. Anagrams contain the same letters with the same counts, so they produce identical keys.
- Add each string to the vector associated with its key, then collect the grouped vectors into the answer.

TC: $O(n \cdot k)$ on average, where $n$ is the number of strings and $k$ is the maximum string length.
SC: $O(n \cdot k)$ for the grouped strings and hash map.

**Code**:
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>>mp;
        vector<vector<string>>ans;
        int n = strs.size();
        for(int i = 0 ; i < n ; i++){
            // Create a key imitating 26 alphabets and there counts
            string key(26, 0);
            for(int j = 0 ; j < strs[i].size() ; j++)   key[strs[i][j] - 'a']++;
            // Anagrams will have the same key
            mp[key].push_back(strs[i]);
        }
        for(auto i = mp.begin() ; i != mp.end() ; i++){
            ans.push_back(i->second);
        }
        return ans;
    }
};
```