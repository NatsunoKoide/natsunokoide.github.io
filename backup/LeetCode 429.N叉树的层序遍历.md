### 题目
给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。
树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

### 示例
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
输入：root =[1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]

### 常规的bfs遍历没什么说的，相当于把传统二叉树遍历的leftright 换成 遍历所有children

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
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
        if(!root) return res;
        queue<Node*> q;
        q.push(root);

        while(!q.empty())
        {
            int n = q.size();
            vector<int> level;
            while(n--)
            {
                auto node = q.front();
                q.pop();
                level.push_back(node->val);
                for(Node* child : node->children) q.push(child);
            }
            res.push_back(level);
        }
        return res;
    }
};
```