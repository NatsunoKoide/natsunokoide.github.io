### 题目
给你一个整数 n 表示一棵 满二叉树 里面节点的数目，节点编号从 1 到 n 。根节点编号为 1 ，树中每个非叶子节点 i 都有两个孩子，分别是左孩子 2 * i 和右孩子 2 * i + 1 。
树中每个节点都有一个值，用下标从 0 开始、长度为 n 的整数数组 cost 表示，其中 cost[i] 是第 i + 1 个节点的值。每次操作，你可以将树中 任意 节点的值 增加 1 。你可以执行操作 任意 次。
你的目标是让根到每一个 叶子结点 的路径值相等。请你返回 最少 需要执行增加操作多少次。

注意：
满二叉树 指的是一棵树，它满足树中除了叶子节点外每个节点都恰好有 2 个子节点，且所有叶子节点距离根节点距离相同。
路径值 指的是路径上所有节点的值之和。

### 示例
输入：n = 7, cost = [1,5,2,2,3,3,1]
输出：6
解释：我们执行以下的增加操作：
- 将节点 4 的值增加一次。
- 将节点 3 的值增加三次。
- 将节点 7 的值增加两次。
从根到叶子的每一条路径值都为 9 。
总共增加次数为 1 + 3 + 2 = 6 。
这是最小的答案。

输入：n = 3, cost = [5,3,3]
输出：0
解释：两条路径已经有相等的路径值，所以不需要执行任何增加操作。

代码1——递归思路
```js
class Solution {
public:
    int minIncrements(int n, vector<int>& cost) {
        int res = 0;
        function<int(int)> dfs = [&](int root)
        {
            //防止越界 限定递归停止条件
            if(root * 2 > n) return cost[root - 1];
            
            int left = dfs(2 * root);
            int right = dfs(2 * root + 1);
            //贪心部分 把叶子节点的最大值减去最小值就是局部最优的增加步骤数
            res += max(left,right) - min(left,right);
            //把叶子节点最大的数加到叶子节点的父节点上，至此将父节点转换为新的叶子节点返回出去
            cost[root - 1] += max(left,right);
            return cost[root - 1];
        };
        //从编号1开始遍历
        dfs(1);
        return res;
    }
};
```

代码2——官方题解（利用数组原地更新）
```js
class Solution {
public:
    int minIncrements(int n, vector<int>& cost) {
        int res = 0;
        for(int i = n - 2;i > 0;i -= 2)
        {
            res += abs(cost[i] - cost[i + 1]);
            cost[i / 2] += max(cost[i] , cost[i + 1]);
        }
        return res;
    }
};
```