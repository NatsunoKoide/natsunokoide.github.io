### 题目（144）
给你二叉树的根节点 root ，返回它节点值的 前序 遍历。

### 示例
输入：root = [1,null,2,3]
输出：[1,2,3]
输入：root = []
输出：[]
输入：root = [1]
输出：[1]

### 总结关键点
1.将遍历单独封装成一个函数，其中res容器数组用引用传入，在主函数调用时可以避免对每一个递归进行添加操作
2.二叉树和N叉树的区别仅仅在于二叉树是对左右递归 ，N叉树是递归儿子的数量

> 代码
```js
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
    void preorder(TreeNode* root,vector<int> &res)
    {
        if(root == nullptr) return;
        res.push_back(root->val);
        preorder(root->left,res);
        preorder(root->right,res);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(root,res);
        return res;
    }
};
```

### 题目（589）
给定一个 n 叉树的根节点  root ，返回 其节点值的 前序遍历 。
n 叉树 在输入中按层序遍历进行序列化表示，每组子节点由空值 null 分隔（请参见示例）。

> 代码
```js
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    void helper(Node* root,vector<int> &res)
    {
        if(root == nullptr) return;
        res.push_back(root->val);
        for(auto child : root->children) helper(child,res);
    }

    vector<int> preorder(Node* root) {
        vector<int> res;
        helper(root,res);
        return res;
    }
};
```
