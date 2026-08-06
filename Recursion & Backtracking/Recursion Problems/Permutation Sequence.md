**Question**: [Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)

**Approach**:
- Think of all permutations as grouped by their first digit; each group has size $(n-1)!$.
- Convert $k$ to 0-based indexing and repeatedly pick the digit at position $k / (n-1)!$ from the remaining numbers.
- Remove the chosen digit, update $k$ to $k \% (n-1)!$, and continue on the smaller remaining set.
- We use $k \% (n-1)!$ because once we pick one digit, the earlier full blocks of permutations for the current position are no longer relevant; we only need the position of the answer inside the remaining block of permutations.
- This builds the answer one position at a time without generating all permutations.

TC: $O(n^2)$ due to repeated removal from a vector and shifting of elements.

SC: $O(n)$ for the remaining numbers and the output string.

**Code**:
```cpp
class Solution {
public:
    string getPermutation(int n, int k) {
        // find the number for each position mathematically
        if(n == 1)  return "1";
        int maxFact = 1;
        vector<int>num{1};
        for(int i = 2 ; i < n ; i++){
            num.push_back(i);
            maxFact *= i;
        }
        num.push_back(n);
        string ans = "";
        k--; // since this is position, we need index.
        
        while(true){
            int idx = k / maxFact; // Get the position of required num
            ans += to_string(num[idx]);
            num.erase(num.begin()+idx); // Remove it now
            if(num.size() == 0) break;
            /*
                We've selected the current digit, so ignore all previous permutation blocks.
                
                Example:
                n = 4, k = 3 (0-based index = 2)
                
                1234
                1243
                1324 <- desired permutation
                1342
                1423
                1432
                
                Since the answer starts with '1', we now only care about the
                permutations of {2,3,4}:
                
                234
                243
                324
                342
                423
                432
                
                Our desired permutation was the 2nd (0-based) permutation within
                the block starting with '1', which is exactly:
                    new_k = old_k % blockSize
                
                So after fixing one digit, k becomes its index inside the selected block.
            */
            k %= maxFact;
            maxFact /= num.size();
        }
        return ans;
    }
};
```