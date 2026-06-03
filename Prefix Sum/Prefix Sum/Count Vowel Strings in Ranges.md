**Question**: [Count Vowel Strings in Ranges](https://leetcode.com/problems/count-vowel-strings-in-ranges/)


**Approach**:
Here, the brute force will be to just follow as the question states. Just check each word for the given query.

A new approach can be to calculate the prefix sum (sum will be number of words that satisfies the condition on the left of the current word).

TC: `O(n)`
SC: `O(n)`


**Code**:
```cpp
class Solution {
public:
    bool isVowel(char c){
        vector<char>vowel = {'a', 'e', 'i', 'o', 'u'};
        for(char x : vowel){
            if(c == x)  return true;
        }
        return false;
    }

    bool isVowelString(string s){
        int l = s[0], r = s.back();
        return isVowel(l) && isVowel(r);
    }

    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        vector<int>pref{0};
        int n = words.size();
        for(int i = 1 ; i <= n ; i++){
            pref.push_back(isVowelString(words[i-1]) + pref.back());
        }
        vector<int>ans;
        for(auto &v : queries){
            int l = v[0], r = v[1];
            int res = pref[r+1] - pref[l];
            ans.push_back(res);
        }

        return ans;
    }
};
```