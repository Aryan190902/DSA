**Question**: [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

**Approach**:
- Do a post-order DFS from the root.
- If the current node is `NULL`, or it matches either target node, return it immediately.
- Recurse into the left and right subtrees and collect the two results.
- If both children return non-`NULL` nodes, the current node is the LCA because it sits above both targets.
- Otherwise, return the non-`NULL` child result if one side found a target, else `NULL`.

TC: `O(n)`, since each node is visited at most once.
SC: `O(h)` auxiliary space for the recursion stack, where `h` is the height of the tree; in the worst case `h = n`.

**Code**:
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* solve(TreeNode* root, TreeNode* p, TreeNode* q) {
        // If we hit the end or one of the target nodes, propagate that node upward.
        if (root == NULL || root == p || root == q) return root;

        // Search both subtrees for the target nodes.
        TreeNode* left = solve(root->left, p, q);
        TreeNode* right = solve(root->right, p, q);

        // If both sides found a target, the current node is the LCA.
        if (left != NULL && right != NULL) return root;

        // Otherwise, return the side that found a target (or NULL if neither did).
        return left != NULL ? left : right;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return solve(root, p, q);
    }
};
```