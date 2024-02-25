### 题目
给定二叉搜索树的根结点 root，返回值位于范围 [low, high] 之间的所有结点的值的和。

### 示例
输入：root = [10,5,15,3,7,null,18], low = 7, high = 15
输出：32
输入：root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
输出：23

### 分析
1.利用bfs的广度询问所有的节点，把所有不满足条件的节点排除，将满足条件的节点进行sum计算然后再把它的子节点放入队列中。
2.还可以使用递归的方法 把满足条件的递归根值和左右范围加起来也能得到最终的答案，但是需要加强对树的理解。

> 代码1——分析1的思路
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
    int rangeSumBST(TreeNode* root, int low, int high) {
        int sum = 0;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty())
        {
            TreeNode* node = q.front();q.pop();
            if(node == nullptr) continue;
            if(node->val > high) q.push(node->left);
            else if(node->val < low) q.push(node->right);
            else{
                sum += node->val;
                q.push(node->left);
                q.push(node->right);
            }
        }
        return sum;
    }
};
```

> 代码2——递归法
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
    int rangeSumBST(TreeNode* root, int low, int high) {
        if(root == nullptr)  return 0;
        if(root->val > high) return rangeSumBST(root->left,low,high);
        if(root->val < low)  return rangeSumBST(root->right,low,high);
        return root->val + rangeSumBST(root->left,low,high) + rangeSumBST(root->right,low,high);
    }
};
```