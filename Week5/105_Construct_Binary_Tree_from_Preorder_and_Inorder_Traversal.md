# 105. Construct Binary Tree from Preorder and Inorder Traversal

```C++
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
    unordered_map<int, int> inorderMap;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i=0; i<inorder.size(); i++){
            inorderMap[inorder[i]] = i;
        }

        return build(preorder, 0, preorder.size() - 1,
                    inorder, 0, inorder.size() - 1);
    }

    TreeNode* build(vector<int>& preorder, int preStart, int preEnd,
                    vector<int>& inorder, int inStart, int inEnd){

        if (preStart > preEnd || inStart > inEnd) return nullptr;

        int rootVal = preorder[preStart];
        TreeNode* root = new TreeNode(rootVal);

        int root_inorderIndex = inorderMap[rootVal];

        int leftSize = root_inorderIndex - inStart;

        root->left = build(preorder, preStart + 1, preStart + leftSize,
                           inorder, inStart, root_inorderIndex - 1);  // build left subtree
        root->right = build(preorder, preStart + leftSize + 1, preEnd,
                    inorder, root_inorderIndex + 1, inEnd);               // build right subtree

        return root;
    }
};
```