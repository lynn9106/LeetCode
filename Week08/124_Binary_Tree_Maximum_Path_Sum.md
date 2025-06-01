# 124. Binary Tree Maximum Path Sum

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
    int maxSum; //recording the max path sum

    int maxPathSum(TreeNode* root) {
        maxSum = INT_MIN;
        dfs(root);      //dfs from root
        return maxSum;  
    }

private:
    // dfs will return the max value path of extending one side
    int dfs(TreeNode* node) {
        if (!node) return 0;

        int leftGain  = max(0, dfs(node->left));    // value of left subtree, if its negative, take 0 and don't extend that way
        int rightGain = max(0, dfs(node->right));  // value of right subtree, if its negative, take 0 and don't extend that way

        int priceNewPath = node->val + leftGain + rightGain;    // new whole path is from left to node to right
        maxSum = max(maxSum, priceNewPath); // update the max sum

        return node->val + max(leftGain, rightGain);    // return one side extension to parent (parent can only go one side)
    }


};
```