### 题目（102）
给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。

### 输出示例
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
输入：root = [1]
输出：[[1]]
输入：root = []
输出：[]

### 复习 DFS 和 BFS

> DFS

```JS
void dfs(TreeNode root) {
    if (root == null) {
        return;
    }
    dfs(root.left);
    dfs(root.right);
}
```

> BFS
```js
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll(); // Java 的 pop 写作 poll()
        if (node.left != null) {
            queue.add(node.left);
        }
        if (node.right != null) {
            queue.add(node.right);
        }
    }
}
```
### 分析
1.二叉树问题一般考虑使用Dfs和Bfs来进行遍历处理。
可以通过模拟注意到 Bfs 在处理层序遍历和最短距离问题上有着良好的遍历优势。

2.针对本题的输出 使用了 vector<vector<int>>这个数据结构，需要注意必须先添加一个空的内部vector数组才能使用 不然编译器不知道内部push_back到哪里
此外queue的用法 push 和 pop  注意不是push_back!!!!!!!!!!!!

### 102代码
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if(!root) return res;
        q.push(root);
        while(!q.empty())
        {
            int currentLevelSize = q.size();
            //res 是一个数组套数组的结构 
            //根据输出结果判断 每一次循环 需要开一个空的数组容器用于循环内部的添加val
            res.push_back(vector<int>());
            for(int i = 0;i < currentLevelSize;i++)
            {
                auto node = q.front();q.pop();
                //先取res的尾部空数组容器，在其中添加node.val
                res.back().push_back(node->val);
                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);
            }
        }
        return res;
    }
};
```

### 题目（103）
给你二叉树的根节点 root ，返回其节点值的 锯齿形层序遍历 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

### 分析
第一种可以使用下方代码 采用 双端队列的方式 结合表示符bool  控制每个阶段的入队顺序，从而输出res
第二种直接对树进行常规层序遍历，在最外层通过for循环 让偶数层的 输出进行reverse也可以达到效果

### 103代码
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        //双端队列 + 逻辑判断处理 输出顺序
        //deque<int> levelList; 但是不能写在这里，因为每一次循环deque需要清空 可以直接写在while循环里自动重新创建
        bool isOrderLeft = true;
        if(!root) return res;
        q.push(root);
        while(!q.empty())
        {
            deque<int> levelList;
            int currentLevelSize = q.size();
            for(int i = 0;i < currentLevelSize;i++)
            {
                auto node = q.front();q.pop();
                if(isOrderLeft) levelList.push_back(node->val);
                else levelList.push_front(node->val);
                if(node->left) q.push(node->left);
                if(node->right)q.push(node->right);
            }
            res.push_back(vector<int>{levelList.begin(),levelList.end()});
            isOrderLeft = !isOrderLeft;
        }
        return res;
    }
};
```