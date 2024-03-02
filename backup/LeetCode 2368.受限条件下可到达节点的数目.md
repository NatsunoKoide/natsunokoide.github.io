### 题目
现有一棵由 n 个节点组成的无向树，节点编号从 0 到 n - 1 ，共有 n - 1 条边。
给你一个二维整数数组 edges ，长度为 n - 1 ，其中 edges[i] = [ai, bi] 表示树中节点 ai 和 bi 之间存在一条边。另给你一个整数数组 restricted 表示 受限 节点。
在不访问受限节点的前提下，返回你可以从节点 0 到达的 最多 节点数目。
注意，节点 0 不 会标记为受限节点。

### 示例
输入：n = 7, edges = [[0,1],[1,2],[3,1],[4,0],[0,5],[5,6]], restricted = [4,5]
输出：4
解释：上图所示正是这棵树。
在不访问受限节点的前提下，只有节点 [0,1,2,3] 可以从节点 0 到达。

输入：n = 7, edges = [[0,1],[0,2],[0,5],[0,4],[3,2],[6,5]], restricted = [4,2,1]
输出：3
解释：上图所示正是这棵树。
在不访问受限节点的前提下，只有节点 [0,5,6] 可以从节点 0 到达。

> 代码 1 —— dfs 遍历 + 去除 限制点
```js
class Solution {
public:
    int reachableNodes(int n, vector<vector<int>>& edges, vector<int>& restricted) {
        vector<int> isrestricted(n,0);
        for(auto &x : restricted)
        {
            isrestricted[x] = 1;
        }

        vector<vector<int>> q(n);
        for(auto &v : edges)
        {
            q[v[0]].push_back(v[1]);
            q[v[1]].push_back(v[0]);
        }
        int res = 0;
        function<void(int,int)> dfs = [&](int x,int f)
        {
            res++;
            for(auto &y : q[x])
            {
                //y != f 防止图遍历的时候重复遍历自己的父节点
                if(y != f && !isrestricted[y]) dfs(y , x);
            }
        };
        dfs(0,-1);
        return res;
    }
};
```