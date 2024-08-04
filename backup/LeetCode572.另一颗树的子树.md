### 题目
给你两棵二叉树 root 和 subRoot 。检验 root 中是否包含和 subRoot 具有相同结构和节点值的子树。如果存在，返回 true ；否则，返回 false 。
二叉树 tree 的一棵子树包括 tree 的某个节点和这个节点的所有后代节点。tree 也可以看做它自身的一棵子树。

### 解题思路
1.首先 常规来看 肯定是需要对root树进行深度遍历获取其子树的
2.其次 针对子树的匹配方式 （1）采取编写check函数对每一个节点的子树都与subroot树匹配 （2）采用kmp算法进行模板匹配（kmp算法忘记怎么写了）
3.最后选择2-（1）的方法进行求解

### 代码
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
    //查两个树节点顺沿下去的子树有没有完全相同
    bool check(TreeNode *o,TreeNode *t)
    {
        //遍历没了还没有false 就是 true
        if(!o && !t) return true;
        if((!o && t) || (o && !t) || (o->val != t->val)) return false;
        return check(o->left,t->left) && check(o->right,t->right);
    }

    //深度遍历每个节点 对每个结点做一次check
    bool dfs(TreeNode *o,TreeNode *t)
    {
        if(!o) return false;
        return check(o,t) || dfs(o->left,t) || dfs(o->right,t);
    }

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        return dfs(root,subRoot);
    }
};
```