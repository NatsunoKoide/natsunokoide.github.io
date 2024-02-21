### 题目
给定两个整数数组 inorder 和 postorder ，其中 inorder 是二叉树的中序遍历， postorder 是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。

### 示例
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
输入：inorder = [-1], postorder = [-1]
输出：[-1]

> 代码（代码随想录）

> 采用vector管理每一次递归的左子树和右子树
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
    TreeNode* traversal (vector<int>& inorder,vector<int>& postorder)
    {
        if(postorder.size() == 0) return NULL;
        //找到根节点 是 后续遍历的最后一个
        int rootValue = postorder[postorder.size()-1];
        TreeNode* root = new TreeNode(rootValue);

        if(postorder.size() == 1) return root;
        
        //这个变量必须定义在for循环外面不然下面的代码使用不了
        int delimiterIndex;
        for(delimiterIndex = 0;delimiterIndex < inorder.size();delimiterIndex++)
        {
            if (inorder[delimiterIndex] == rootValue) break;
        }
        //左闭右开原则
        vector<int> leftInorder(inorder.begin(),inorder.begin() + delimiterIndex);
        vector<int> rightInorder(inorder.begin() + delimiterIndex + 1,inorder.end());

        postorder.resize(postorder.size() - 1);

        vector<int> leftPostorder(postorder.begin(),postorder.begin() + leftInorder.size());
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(),postorder.end());

        root->left = traversal(leftInorder,leftPostorder);
        root->right = traversal(rightInorder,rightPostorder);

        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {    
        if(inorder.size() == 0 || postorder.size() == 0) return nullptr;
        return traversal(inorder,postorder);
    }
};
```

> 代码（自码）
```js

```