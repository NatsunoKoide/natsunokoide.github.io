### 题目
给你一个 二叉搜索树 的根节点 root ，和一个由正整数组成、长度为 n 的数组 queries 。
请你找出一个长度为 n 的 二维 答案数组 answer ，其中 answer[i] = [mini, maxi] ：
mini 是树中小于等于 queries[i] 的 最大值 。如果不存在这样的值，则使用 -1 代替。
maxi 是树中大于等于 queries[i] 的 最小值 。如果不存在这样的值，则使用 -1 代替。
返回数组 answer 。

### 示例
输入：root = [6,2,13,1,4,9,15,null,null,null,null,null,null,14], queries = [2,5,16]
输出：[[2,2],[4,6],[15,-1]]
解释：按下面的描述找出并返回查询的答案：
- 树中小于等于 2 的最大值是 2 ，且大于等于 2 的最小值也是 2 。所以第一个查询的答案是 [2,2] 。
- 树中小于等于 5 的最大值是 4 ，且大于等于 5 的最小值是 6 。所以第二个查询的答案是 [4,6] 。
- 树中小于等于 16 的最大值是 15 ，且大于等于 16 的最小值不存在。所以第三个查询的答案是 [15,-1] 。

输入：root = [4,null,9], queries = [3]
输出：[[-1,4]]
解释：树中不存在小于等于 3 的最大值，且大于等于 3 的最小值是 4 。所以查询的答案是 [-1,4] 。

### 知识点
1.二叉搜索树 采用 dfs +  中序遍历 可以得到有序的vector数组内容。
2.C++中的lower_bound函数，它用于在有序容器（如vector或array）中查找第一个不小于给定值的元素的位置，并返回一个迭代器指向这个位置。
3.定义maxval和minval为-1，当指针没有超过末尾的时候找到了符合要求的迭代器 将迭代器的值赋给maxval,同时如果这个迭代器的值 与val相同 则将minval也赋予这个值，此外当it不等于0且迭代器的值又不等于val时，minval等于迭代器-1位置上的数。若迭代器等于起始位置，或者迭代器等于结尾位置，证明没有符合要求的迭代器返回 -1 -1.

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
    vector<vector<int>> closestNodes(TreeNode* root, vector<int>& queries) {
        //queue<TreeNode*> q;
        vector<vector<int>> res;
        vector<int> vec;
        function<void(TreeNode*)> dfs = [&](TreeNode *root) {
            if (!root) {
                return;
            }    
            dfs(root->left);
            vec.emplace_back(root->val);
            dfs(root->right);
        };
        dfs(root);
        //本体采用bfs进行数据存储会爆内存
        //可见 dfs在树的常规遍历里更加简单且内存少
        // q.push(root);
        // while(!q.empty())
        // {
        //     TreeNode* node = q.front();q.pop();
        //     vec.push_back(node->val);
        //     if(root->left) q.push(root->left);
        //     if(root->right)q.push(root->right);
        // }
        
        for(int val : queries)
        {
            int maxVal = -1,minVal = -1;
            //lower_bound函数，它用于在有序容器（如vector或array）
            //中查找第一个不小于（大于等于）给定值的元素的位置，并返回一个迭代器指向这个位置
            auto it = lower_bound(vec.begin(),vec.end(),val);
            if(it != vec.end())
            {
                maxVal = *it;
                if(*it == val)
                {
                    //如果这个指针指向的数和自己一样 就把min也赋予这个值
                    minVal = *it;
                    res.push_back({minVal,maxVal});
                    continue;
                }
            }
            //若迭代器大于0，同时保证下面--it不为空 小于等于val的最大元素就是it的前面一个迭代器指向
            if(it != vec.begin())
            {
                minVal = *(--it);
            }
            res.push_back({minVal,maxVal});
        }
        return res;
    }
};
```