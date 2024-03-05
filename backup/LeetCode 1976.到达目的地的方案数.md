### 题目
你在一个城市里，城市由 n 个路口组成，路口编号为 0 到 n - 1 ，某些路口之间有 双向 道路。输入保证你可以从任意路口出发到达其他任意路口，且任意两个路口之间最多有一条路。
给你一个整数 n 和二维整数数组 roads ，其中 roads[i] = [ui, vi, timei] 表示在路口 ui 和 vi 之间有一条需要花费 timei 时间才能通过的道路。你想知道花费 最少时间 从路口 0 出发到达路口 n - 1 的方案数。
请返回花费 最少时间 到达目的地的 路径数目 。由于答案可能很大，将结果对 109 + 7 取余 后返回。

### 示例
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/cad0b401-7c4d-4fa2-9b50-479237f01abf)
输入：n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
输出：4
解释：从路口 0 出发到路口 6 花费的最少时间是 7 分钟。
四条花费 7 分钟的路径分别为：
- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

输入：n = 2, roads = [[1,0,10]]
输出：1
解释：只有一条从路口 0 到路口 1 的路，花费 10 分钟。

> 代码
```js
class Solution {
public:
    using LL = long long;
    int countPaths(int n, vector<vector<int>>& roads) {
        const long long mod = 1e9 + 7;
        vector<vector<pair<int,int>>> e(n);
        //把roads里面的边数据 构建成图
        for(const auto& road : roads)
        {
            int x = road[0],y = road[1],t = road[2];
            e[x].emplace_back(y,t);
            e[y].emplace_back(x,t);
        }
        vector<LL> dis(n,LLONG_MAX);
        //ways[v] 就表示源到点i最短的路径数目
        vector<LL> ways(n);
    
        priority_queue<pair<LL, int>, vector<pair<LL, int>>, greater<pair<LL, int>>> q;
        //初始化
        q.emplace(0,0);
        dis[0] = 0;
        ways[0] = 1;

        while(!q.empty())
        {
            auto [t,u] = q.top();
            q.pop();
            //不是最路径直接下一个
            if(t > dis[u]) continue;
            //从0开始bfs 取出链接的node
            for(auto& [v,w] : e[u])
            {
                if(t + w < dis[v])
                {
                    dis[v] = t + w;
                    ways[v] = ways[u];
                    q.emplace(t+w,v);
                }
                else if(t + w == dis[v])
                {
                    ways[v] = (ways[u] + ways[v]) % mod;
                }
            }
        }
        return ways[n - 1];
    }
};
```
