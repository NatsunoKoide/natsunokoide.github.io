### 题目
给定一个 R行 C列的矩阵，表示一个矩形网格滑雪场。
矩阵中第 i 行第 j列的点表示滑雪场的第 i行第 j列区域的高度。
一个人从滑雪场中的某个区域内出发，每次可以向上下左右任意一个方向滑动一个单位距离。
当然，一个人能够滑动到某相邻区域的前提是该区域的高度低于自己目前所在区域的高度。
现在给定你一个二维矩阵表示滑雪场各区域的高度，请你找出在该滑雪场中能够完成的最长滑雪轨迹，并输出其长度(可经过最大区域数)。

### 输入格式
第一行包含两个整数 R 和 C。
接下来 R行，每行包含 C个整数，表示完整的二维矩阵。

### 输出格式
输出一个整数，表示可完成的最长滑雪长度。

### 数据范围
1≤R,C≤300,
0≤矩阵中整数≤10000

### 考点
记忆化搜索

### 核心dp思路
简而言之就是 将每个点都当作起点 去 遍历所有格子 满足条件的格子就+1 递归结束后就能得到最max的值
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/1eb86ed0-194f-4599-805f-b458588463a8)


> 代码
```js
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 310;
int n,m;
int f[N][N],h[N][N];

int dx[4] = {-1,0,1,0}, dy[4] = {0,1,0,-1};

int dp(int x,int y)
{
    if(f[x][y] != -1) return f[x][y]; //不等于-1 证明更新过了
    f[x][y] = 1; //数组初始化，至少有一个长度
    for(int i = 0;i < 4;i++)
    {
        int a = x + dx[i],b = y + dy[i];
        if(a >= 1 && a <= n && b >= 1 && b <= m && h[a][b] < h[x][y])
        {
            //这里的dp(a,b)相当于dfs遍历递归所有相邻的格子
            //满足要求的化就 + 1 
            f[x][y] = max(f[x][y],dp(a,b) + 1);
        }
    }
    return f[x][y];
}

int main()
{
    cin >> n >> m;
    for(int i = 1;i <=n ;i++)
        for(int j = 1;j <= m;j++)
            cin >> h[i][j];
    
    memset(f,-1,sizeof f);
    int res = 0;
    for(int i = 1;i <=n ;i++)
        for(int j = 1;j <= m;j++)
            res = max (res,dp(i,j));
    cout << res << endl;
    return 0;
}
```