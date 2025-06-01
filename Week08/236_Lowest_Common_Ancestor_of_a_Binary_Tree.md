# 236. Lowest Common Ancestor of a Binary Tree

```C++
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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q){         // if find p or q, return p or q, and if is null return null
            return root;
        }

        TreeNode* left = lowestCommonAncestor(root->left, p, q);    // check if find in left subtree
        TreeNode* right = lowestCommonAncestor(root->right, p, q);  // check if find in right subtree

        if(left && right) return root;      // if one find in left, one find in right, then the root is the common ancestor
        return left ? left : right;         // if there's only one side, the last return root is the common ancestor
    }
};
```