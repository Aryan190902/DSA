**Question**: [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

**Approach**:
- Traverse the tree with DFS and maintain a valid value range for each node.
- Every node must satisfy `m < node->val < M`, where `m` and `M` are the allowed lower and upper bounds from its ancestors.
- Recurse left with `(m, root->val)` and recurse right with `(root->val, M)` so the BST ordering invariant is enforced at every level.
- This handles duplicate values correctly by using strict comparisons, and avoids integer overflow by passing the bounds as `long` values.

TC: `O(n)`, because each node is visited exactly once.
SC: `O(h)` auxiliary space for the recursion stack, where `h` is the tree height; worst case is `O(n)` for a skewed tree.

**Code**:
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool solve(TreeNode* root, long m, long M) {
        if (!root) return true;

        int val = root->val;
        // Each node must stay strictly inside its ancestor range.
        if (val <= m || val >= M) return false;

        // Left subtree values must be less than current value; right subtree values must be greater.
        bool left = root->left != NULL ? solve(root->left, m, root->val) : true;
        bool right = root->right != NULL ? solve(root->right, root->val, M) : true;

        return left && right;
    }

    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        // Use extended bounds to allow the full int range without overflow.
        return solve(root, long(INT_MIN) - 1, long(INT_MAX) + 1);
    }
};
```