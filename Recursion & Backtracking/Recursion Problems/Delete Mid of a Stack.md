**Question**: [Delete Mid of a Stack](https://www.geeksforgeeks.org/problems/delete-middle-element-of-a-stack/1)

**Approach**:
- Just reach the mid, pop the element, and then push the remaining elements back.

TC: `O(n)`
SC: `O(1)`

**Code**:
```cpp
class Solution {
  public:
    // Function to delete middle element of a stack.
    void deleteMid(stack<int>& s) {
        // code here..
        int n = s.size();
        int mid = n/2;
        stack<int>tmp;
        while(mid >= 0){
            int val = s.top();
            s.pop();
            tmp.push(val);
            mid--;
        }
        tmp.pop();
        while(!tmp.empty()){
            int val = tmp.top();
            tmp.pop();
            s.push(val);
        }
    }
};
```