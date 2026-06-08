**Question**: [Product of Last K integers](https://leetcode.com/problems/product-of-last-k-integers/)

**Approach**:
Need to create a `pref` array, the product of Last K integers will be `pref[n-1] / pref[n-1-k]`.

TC: `O(n)`
SC: `O(n)`

**Code**:

```cpp
class ProductOfNumbers {
public:
    vector<int>pref;
    ProductOfNumbers() {
        pref.push_back(1);
    }
    
    void add(int num) {
        if(num == 0){
            pref.clear();
            pref.push_back(1);
            return;
        }
        pref.push_back(pref.back()*num);
    }
    
    int getProduct(int k) {
        int n = pref.size();
        if(k >= n)  return 0;
        return pref[n-1] / pref[n-1-k];
    }
};
```