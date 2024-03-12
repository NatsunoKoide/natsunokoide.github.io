### 题目
给出一个满足下述规则的二叉树：

1.root.val == 0
2.如果 treeNode.val == x 且 treeNode.left != null，那么 treeNode.left.val == 2 * x + 1
   如果 treeNode.val == x 且 treeNode.right != null，那么 treeNode.right.val == 2 * x + 2
4.现在这个二叉树受到「污染」，所有的 treeNode.val 都变成了 -1。

请你先还原二叉树，然后实现 FindElements 类：
FindElements(TreeNode* root) 用受污染的二叉树初始化对象，你需要先把它还原。
bool find(int target) 判断目标值 target 是否存在于还原后的二叉树中并返回结果。
 
### 示例
输入：
["FindElements","find","find"]
[[[-1,null,-1]],[1],[2]]
输出：
[null,false,true]
解释：
FindElements findElements = new FindElements([-1,null,-1]); 
findElements.find(1); // return False 
findElements.find(2); // return True 

输入：
["FindElements","find","find","find"]
[[[-1,-1,-1,-1,-1]],[1],[3],[5]]
输出：
[null,true,true,false]
解释：
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]);
findElements.find(1); // return True
findElements.find(3); // return True
findElements.find(5); // return False

> 代码（bfs+set计数）
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
class FindElements {
public:
    unordered_set<int> S;
    FindElements(TreeNode* root) {
        if(root != nullptr) root -> val = 0;
        queue<TreeNode*> q; 
        S.insert(0);
        q.push(root);
        while(!q.empty())
        {
            int n = q.size();
            for(int i = 0;i < n;i++)
            {
                TreeNode* node = q.front();q.pop();
                if( node->left != nullptr)
                {
                    node->left->val = node->val * 2 + 1;
                    S.insert(node->left->val);
                    q.push(node->left);
                }
                if (node->right != nullptr)
                {
                    node->right->val = node->val * 2 + 2;
                    S.insert(node->right->val);
                    q.push(node->right);
                }
            }
        }

    }
    
    bool find(int target) {
        return S.count(target) > 0;
    }
};

/**
 * Your FindElements object will be instantiated and called as such:
 * FindElements* obj = new FindElements(root);
 * bool param_1 = obj->find(target);
 */
```

> 代码2(dfs+计数)(官方题解)
```js
class FindElements {
private:
    unordered_set<int> valSet;

    void dfs(TreeNode *node, int val) {
        if (node == nullptr) {
            return;
        }
        node->val = val;
        valSet.insert(val);
        dfs(node->left, val * 2 + 1);
        dfs(node->right, val * 2 + 2);
    }

public:
    FindElements(TreeNode* root) {
        dfs(root, 0);
    }

    bool find(int target) {
        return valSet.count(target) > 0;
    }
};
```