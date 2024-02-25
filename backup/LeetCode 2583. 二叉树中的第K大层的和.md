### 题目
给你一棵二叉树的根节点 root 和一个正整数 k 。
树中的 层和 是指 同一层 上节点值的总和。
返回树中第 k 大的层和（不一定不同）。如果树少于 k 层，则返回 -1 。
注意，如果两个节点与根节点的距离相同，则认为它们在同一层。

### 示例
输入：root = [5,8,9,2,1,3,7,4,6], k = 2
输出：13
解释：树中每一层的层和分别是：
- Level 1: 5
- Level 2: 8 + 9 = 17
- Level 3: 2 + 1 + 3 + 7 = 13
- Level 4: 4 + 6 = 10
第 2 大的层和等于 13 。

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
    long long kthLargestLevelSum(TreeNode* root, int k) {
        vector<long long> res;
        queue<TreeNode*> q;
        if(root == nullptr) return 0;
        q.push(root);
        while(!q.empty())
        {
		int levelSize = q.size();
		long long levelSum = 0;
		for(int i = 0;i < levelSize;i++)
	        {
		TreeNode* node = q.front();q.pop();
                levelSum += node->val;
                if(node->left) q.push(node->left);
                if(node->right)q.push(node->right);
                }
		res.push_back(levelSum);
        }
        if(res.size() < k)return -1;
        sort(res.begin(),res.end());
        return res[res.size() - k];
    }
};
```