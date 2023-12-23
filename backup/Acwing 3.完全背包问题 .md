### 完全背包问题
**有 N种物品和一个容量是 V的背包，每种物品都有无限件可用。第 i种物品的体积是 vi，价值是 wi。
求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。输出最大价值。**

> 输入格式
**第一行两个整数，N，V，用空格隔开，分别表示物品数量和背包容积。接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 件物品的体积和价值。**

> 输出格式
**输出一个整数，表示最大价值。**

> 数据范围
0<N,V≤1000
0<vi,wi≤1000

### 朴素解法
_朴素解法中采用三层循环，相较于01背包多了一层系数k的循环，这个系数k用于代表多少个当前物体（因为完全背包问题物品是无限）_

> 代码 （在Acwing平台 出现 TLE）
```js
#include <iostream>
using namespace std;
const int N = 1010;
int f[N][N];
int v[N],w[N];

int main()
{
    int n,m;
    cin >> n >> m;
    for(int i = 1;i <= n;i++)
    {
        cin >> v[i] >> w[i];
    }
    for(int i = 1;i <= n;i++)
    {
        for(int j = 1;i <= m;j++)
        {
            for(int k = 0;k * v[i] <= j;k++)
            {
                f[i][j] = max(f[i][j],f[i-1][j - k*v[i]] + k*w[i]);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

### 二维优化（依据递推关系）

> 递推关系分析
f[i , j ] = max( f[i-1,j] , f[i-1,j-v]+w ,  f[i-1,j-2*v]+2*w , f[i-1,j-3*v]+3*w , .....)
f[i , j-v]= max(            f[i-1,j-v]   ,  f[i-1,j-2*v] + w , f[i-1,j-3*v]+2*w , .....)
由上两式，可得出如下递推关系：   f[i][j]=max(f[i,j-v]+w , f[i-1][j]) 

_根据递推关系修改代码可以发现 完全背包问题的核心代码与01背包问题的代码只有容量的顺序不同_

> 0-1背包的核心代码
```js
for(int i = 1 ; i <= n ; i++)
for(int j = 0 ; j <= m ; j ++)
{
    f[i][j] = f[i-1][j];
    if(j-v[i]>=0)
        f[i][j] = max(f[i][j],f[i-1][j-v[i]]+w[i]);
}
```

> 完全背包问题的核心代码
```js
for(int i = 1 ; i <=n ;i++)
for(int j = 0 ; j <=m ;j++)
{
    f[i][j] = f[i-1][j];
    if(j-v[i]>=0)
        f[i][j]=max(f[i][j],f[i][j-v[i]]+w[i]);
}
```

> 二维优化后的代码
```js
#include <iostream>
using namespace std;
const int N = 1010;
int f[N][N];
int v[N],w[N];

int main()
{
    int n,m;
    cin >> n >> m;
    for(int i = 1;i <= n;i++)
    {
        cin >> v[i] >> w[i];
    }
    for(int i = 1;i <= n;i++)
    {
        for(int j = 1;j <= m;j++)
        {
            /*for(int k = 0;k * v[i] <= j;k++)
            {
                f[i][j] = max(f[i][j],f[i-1][j - k*v[i]] + k*w[i]);
            }*/
            //通过递推关系进行算式优化
            f[i][j] = f[i-1][j];
            if(j-v[i]>=0)
            {
                f[i][j] = max(f[i][j],f[i-1][j-v[i]]+w[i]);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```
### 一维优化（01背包问题中提及过的一维数组优化）
_相较于01背包问题，唯一的区别在于####号包裹的这个循环是正序的_
```js
#include<iostream>
using namespace std;
const int N = 1010;
int f[N];
int v[N],w[N];
int main()
{
    int n,m;
    cin>>n>>m;
    for(int i = 1 ; i <= n ;i ++)
    {
        cin>>v[i]>>w[i];
    }
    for(int i = 1 ; i<=n ;i++)
//################################
    for(int j = v[i] ; j<=m ;j++)
//################################
    {
        [j] = max(f[j],f[j-v[i]]+w[i]);
    }
    cout<<f[m]<<endl;
}
```
