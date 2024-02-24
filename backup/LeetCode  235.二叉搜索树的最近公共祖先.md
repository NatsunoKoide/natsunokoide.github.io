### 题目
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
[百度百科]中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

### 示例
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。

> 代码1——递归法
```js
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
    TreeNode* traveral(TreeNode* cur,TreeNode* p, TreeNode* q)
    {
        if(cur == NULL) return cur;
        if(cur->val > p->val && cur->val > q->val)
        {
            TreeNode* left = traveral(cur->left,p,q);
            //递归到最后出来的left要返回出去
            if(left != NULL) return left;
        }
        if(cur->val < p->val && cur->val < q->val)
        {
            TreeNode* right = traveral(cur->right,p,q);
            //递归到最后出来的right要返回出去
            if(right != NULL) return right;
        }
        return cur;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* res = traveral(root , p , q);
        return res;
    }
};
```

> 代码2——迭代法
```js
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
        while(root)
        {
            if(root->val > p->val && root->val > q->val) root = root->left;
            else if (root->val < p->val && root->val < q->val) root = root->right;
            else break;
        }
        return root;
    }
};
```
