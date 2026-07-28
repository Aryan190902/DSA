**Question**: [Basic Calculator](https://leetcode.com/problems/basic-calculator/)

**Approach**:
- Since we need to solve the brackets first, we take a stack, and store the ans, and sign.
- When we see a closing paranthese, we know that this part should be solved now, so we calculate the current answer, and add the previous result stored in the stack.
- Finally, we return the total answer.

TC: `O(n)`
SC: `O(n)` for stack.

**Code**:
```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int>st; // For handling parantheses.
        int ans = 0;
        long val = 0;
        int sign = 1; // 1 for pos, -1 for neg

        for(int i = 0 ; i < s.length() ; i++){
            char c = s[i];
            if(c >= '0' && c <= '9'){
                val = val*10 + (c - '0');
            }
            else if(c == '+' || c == '-'){
                ans += val*sign;
                val = 0;
                sign = (c == '+') ? 1 : -1;
            }
            else if(c == '('){
                st.push(ans);
                st.push(sign);
                ans = 0;
                sign = 1;
            }
            else if(c == ')'){
                ans += sign*val;
                val = 0;

                int prevSign = st.top();
                st.pop();

                int prevAns = st.top();
                st.pop();

                ans = prevAns + prevSign*ans;
            }
        }
        ans += sign*val;
        return ans;
    }
};
```