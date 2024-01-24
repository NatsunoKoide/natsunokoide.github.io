### 题目
Ural 大学有 N名职员，编号为 1∼N。
他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。
每个职员有一个快乐指数，用整数 Hi给出，其中 1≤i≤N。
现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。
在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

> 出自算法竞赛进阶指南

### 考点
单链表树，dfs，树状dp

### 输入格式
第一行一个整数 N。
接下来 N行，第 i行表示 i号职员的快乐指数 Hi。
接下来 N−1 行，每行输入一对整数 L,K，表示 K是 L的直接上司。（注意一下，后一个数是前一个数的父节点，不要搞反）。

### 输出格式
输出最大的快乐指数。

### 数据范围
1≤N≤6000,
−128≤Hi≤127

### dp分析
1. 状态方程f[i][1]表示选i号节点。当f[i][0]时表示不选i号节点。
2. 模拟可以的出结论
当不选i节点时，影响最大高兴值的节点为i的子节点或其他节点
当选i节点时，影响最大高兴值的节点只为i的子节点
3.进一步总结为：（1）f[i][1]时加上f[j][0] （2）f[i][0]时加上maxf[j][0],f[j][1]


> 代码
```js
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 6010;
int e[N],ne[N],h[N],idx; //单链表表示树
int f[N][2];
int happy[N];
bool has_father[N];
int n;

//针对单链表的b加入a
void add(int a,int b)
{
    e[idx] = b;ne[idx] = h[a];h[a] = idx++; 
}

//核心函数——树状dp+dfs
void dfs(int u)
{
    f[u][1] = happy[u];//若选了根节点，把根节点的快乐值加入
    for(int i = h[u];i != -1;i = ne[i]) //遍历树 此处！=1 对应memset(h,-1,sizeof h);
    {
        int j = e[i];
        dfs(j);//递归到叶子节点
        //开始状态转移部分
        f[u][1] += f[j][0];
        f[u][0] += max(f[j][1],f[j][0]);
    }
}

int main()
{
    cin >> n;
    for(int i = 1;i <= n;i++) cin >> happy[i];
    memset(h,-1,sizeof h);
    for(int i = 1;i < n;i++)
    {
        int a,b;
        cin >> a >> b;
        has_father[a] = true;
        add(b,a); //把a加到b后面
    }
    int root = 1;
    //因为这个数是有序的1~N所以可以这样判断第几个数是root
    while(has_father[root]) root++;
    dfs(root);
    printf("%d\n",max(f[root][0],f[root][1]));
    return 0;
}
```