**Question**: [Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/)

**Approach**: 
- For each expression, we can divide the string into two parts at every operator found in the string. 
- We recursively solve for the left and right parts, then combine the results using the current operator. 
- We use memoization to store results of sub-expressions to avoid redundant calculations.

TC: $O(n^2*2^n)$ For optimized solution
SC: $O(n^2*2^n)$ recursive stack

**Code**:
```cpp
class Solution {
public:
    map<string, vector<int>>dp;
    int perform(int x, int y, char c){
        if(c == '+')    return x + y;
        else if(c == '-')   return x - y;
        else    return x * y;
    }

    vector<int> diffWaysToCompute(string expression) {
        vector<int>ans;
        // If expression seen before, don't compute and return the result
        if(dp.find(expression) != dp.end())  return dp[expression];

        for(int i = 0 ; i < expression.size() ; i++){
            char c = expression[i];
            if(c == '+' || c == '-' || c == '*'){
                // Get the result of left, and right side of the expression
                vector<int>tmp1 = diffWaysToCompute(expression.substr(0,i)); 
                vector<int>tmp2 = diffWaysToCompute(expression.substr(i+1));
                for(int x : tmp1){
                    for(int y : tmp2){
                        // All the results will be combined based on the current operator
                        ans.push_back(perform(x, y, c));
                    }
                }
            }
        }
        if(ans.empty()) ans.push_back(stoi(expression)); // If there are no operators
        return dp[expression] = ans;
    }
};
```