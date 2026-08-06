**Question**: [Sort a Stack](https://www.geeksforgeeks.org/problems/sort-a-stack/1)

**Approach**:
- Since contrainsts are small (stack size and values up to 1000), we can use a frequency array (counting sort approach) to store the counts of each element and then push them back into the stack in sorted order.

TC: `O(1000)`
SC: `O(1000)`

**Code**:
```cpp
class Solution {
  public:
    void sortStack(stack<int> &st) {
        // code here
        vector<int>tmp(1001, 0);
        while(!st.empty()){
            int val = st.top();
            st.pop();
            tmp[val]++;
        }
        for(int i = 0 ; i < tmp.size() ; i++){
            if(tmp[i] > 0){
                for(int j = 0 ; j < tmp[i] ; j++)   st.push(i);
            }
        }
        
    }
};
```