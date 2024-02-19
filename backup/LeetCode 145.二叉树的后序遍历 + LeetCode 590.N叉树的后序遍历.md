### 题目（145）
给你一棵二叉树的根节点 root ，返回其节点值的 后序遍历 。

### 示例
输入：root = [1,null,2,3]
输出：[3,2,1]
输入：root = []
输出：[]
输入：root = [1]
输出：[1]

###  分析
很常规的遍历问题 注意递归顺序即可


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
    void postorder(TreeNode* root,vector<int> &res)
    {
        if(!root) return;
        postorder(root->left,res);
        postorder(root->right,res);
        res.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        postorder(root,res);
        return res;
    }
};
```

### 题目（590）
给定一个 n 叉树的根节点 root ，返回 其节点值的 后序遍历 。
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
        for(auto child : root->children) helper(child,res);
        res.push_back(root->val);
    }

    vector<int> postorder(Node* root) {
        vector<int> res;
        helper(root,res);
        return res;
    }
};
```