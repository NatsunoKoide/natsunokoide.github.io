### 题目
给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， 边权可能为负数。
请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n  号点，则输出impossible。
数据保证不存在负权回路。

### 输入格式
第一行包含整数 n 和 m。
接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

### 输出格式
输出一个整数，表示 1 号点到 n 号点的最短距离。
如果路径不存在，则输出 impossible。

### 数据范围
1≤n,m≤100000,
图中涉及边长绝对值均不超过 10000。

### 图解spfa
![spfa](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/7d1cf7da-04ed-472d-a3be-b9ff74ec3a90)

### 分析spfa
SPFA算法仅仅只是对Bellman_ford算法的一个优化。
Bellman_ford算法会遍历所有的边，但是有很多的边遍历了其实没有什么意义，我们只用遍历那些到源点距离变小的点所连接的边即可，只有当一个点的前驱结点更新了，该节点才会得到更新；因此考虑到这一点，我们将创建一个队列每一次加入距离被更新的结点。

_Bellman_ford算法可以存在负权回路，是因为其循环的次数是有限制的因此最终不会发生死循环；但是SPFA算法不可以，由于用了队列来存储，只要发生了更新就会不断的入队，因此假如有负权回路请你不要用SPFA否则会死循环。_

> 代码
```js
#include <iostream>
#include <algorithm>
#include <queue>
#include <cstring>

using namespace std;

const int N = 100010;

int n,m;
int h[N],w[N],e[N],ne[N],idx;
int dist[N];
bool st[N];

void add(int a,int b,int c)
{
    e[idx] = b,w[idx] = c,ne[idx] = h[a],h[a] = idx++;
}

int spfa()
{
    memset(dist,0x3f,sizeof dist);
    dist[1] = 0;
    queue<int> q;
    q.push(1);
    st[1] = true;

    while(q.size())
    {
        int t = q.front();
        q.pop();
        st[t] = false;
        for(int i = h[t];i != -1;i = ne[i])
        {
            int j = e[i];
            if(dist[j] > dist[t] + w[i])
            {
                dist[j] = dist[t] + w[i];
                //如果没有被查询过
                if(!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return dist[n];
}

int main()
{
    cin >> n >> m;
    memset(h,-1,sizeof h);
    while(m--)
    {
        int a,b,c;
        cin >> a >> b >> c;
        add(a,b,c);
    }
    int t = spfa();
    if(t == 0x3f3f3f3f) puts("impossible");
    else printf("%d\n",t);
    return 0;
}
```