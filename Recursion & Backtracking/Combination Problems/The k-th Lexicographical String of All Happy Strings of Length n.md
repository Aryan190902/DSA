**Question**: [The k-th Lexicographical String of All Happy Strings of Length n](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/)

**Approach**:

- Compute the total number of happy strings as `3 * 2^(n-1)` and return empty if `k` exceeds it.
- Convert `k` to 0-based index so the math works naturally with groups.
- The first character is chosen from `a`, `b`, `c` using `k / groupSize`, where `groupSize = 2^(n-1)`.
- After fixing the first character, update `k %= groupSize` and halve `groupSize` for each next position.
- For each subsequent position, choose one of the two letters that differ from the previous character.
- Use `k / groupSize` to decide between the two valid next characters, then reduce `k` with `k %= groupSize`.
- Build the string iteratively without generating all strings.

TC: `O(n)`.
SC: `O(n)`.

**Code**:
```cpp
class Solution {
public:

    string getHappyString(int n, int k) {
        vector<string>allStrings;
        string temp = "";
        // Fix first chara, then for each position, only 2 choices left.
        // First character can be a,b,c
        int tot = 3 * (1 << (n-1)); 
        if(k > tot) return "";

        k--; // index instead of position
        // First the first character
        int groupSize = 1 << (n-1);
        char c = 'a' + (k/groupSize);
        k %= groupSize;
        temp += c;
        for(int i = 0 ; i < n-1 ; i++){
            groupSize >>= 1;
            char lastOne = temp.back();
            char op1, op2;
            switch(lastOne){
                case 'a':
                    op1 = 'b', op2 = 'c';
                    break;
                case 'b':
                    op1 = 'a', op2 = 'c';
                    break;
                case 'c':
                    op1 = 'a', op2 = 'b';
                    break;
                default:
                    break;
            }
            if(k/groupSize == 0)    temp += op1; // k <= groupSize
            else    temp += op2;
            k %= groupSize;
        }
        return temp;
    }
};
```