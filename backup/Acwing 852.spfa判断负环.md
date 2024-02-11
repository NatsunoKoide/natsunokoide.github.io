### 题目
给定一个 n个点 m条边的有向图，图中可能存在重边和自环， 边权可能为负数。
请你判断图中是否存在负权回路。

### 输入格式
第一行包含整数 n 和 m。
接下来 m行每行包含三个整数 x,y,z,,，表示存在一条从点 x到点 y 的有向边，边长为 z。

### 输出格式
如果图中存在负权回路，则输出 Yes，否则输出 No。

### 数据范围
点小于等于2000
边数小于等于10000
图中涉及的绝对值均不超过10000

### 分析与注意点
1.相较于spfa求最短路径，spfa函数中不需要对dist初始化（因为不必求精确的距离是多少），需要将所有的点都放入到queue中（因为起点不同可能不会存在负环，需要对所有点进行负环检查）
2.整体思路就是对距离进行最短距离的更新，同时每一次更新对cnt数组++，当图中存在负环，数值和边数的值是会一直更新的（所以说spfa不适用于存在负环的图论问题），那么当cnt >= n 则认定为存在负环。 

> 代码
```js
#include <iostream>
#include <queue>
#include <cstring>
#include <algorithm>

using namespace std;

const int M = 10010,N = 2010;
int n,m;
int h[N],ne[M],e[M],w[M],idx;
int dist[N],cnt[N];
bool st[N];

void add(int a,int b,int c)
{
    e[idx] = b,w[idx] = c,ne[idx] = h[a],h[a] = idx++;
}

bool spfa()
{
    queue<int> q;
    for(int i = 1;i <= n;++i)
    {
        st[i] = true;
        q.push(i);
    }
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
                cnt[j] = cnt[t] + 1;
                if(cnt[j] >= n) return true;
                if(!st[j])
                {
                    q.push(j);
                    st[j] = true;
                }
            }
        }
    }
    return false;
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
    cout << (spfa() ? "Yes" : "No");
    return 0;
}
```