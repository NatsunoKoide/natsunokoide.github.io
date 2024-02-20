### 题目
给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。

### 示例
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
输入: preorder = [-1], inorder = [-1]
输出: [-1]

> 代码（递归法）
```js
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<int, int> index;

    TreeNode* myBuildeTree(const vector<int>& preorder,
                           const vector<int>& inorder, int preorder_left,
                           int preorder_right, int inorder_left,
                           int inorder_right) {
        if (preorder_left > preorder_right)
            return nullptr;
        //前序遍历的第一个节点就是根节点
        int preorder_root = preorder_left;
        //利用哈希表把根节点从中序遍历中找出来
        int inorder_root = index[preorder[preorder_root]];
        //先建立根节点
        TreeNode* root = new TreeNode(preorder[preorder_root]);
        //得到左子树节点数目
        int size_left_subtree = inorder_root - inorder_left;
        //递归构建左子树并连接到根节点
        root->left = myBuildeTree(preorder, inorder, 
                                  preorder_left + 1,preorder_left + size_left_subtree,
                                  inorder_left, inorder_root - 1);
        //递归构建右子树并连接到根节点
        root->right = myBuildeTree(preorder, inorder,
                                   preorder_left + size_left_subtree + 1,
                                   preorder_right, inorder_root + 1, inorder_right);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        //对中序遍历的结果进行哈希映射
        for (int i = 0; i < n; i++)
            index[inorder[i]] = i;
        return myBuildeTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
};
```