### 题目有 N种物品和一个容量是 V的背包。第 i种物品最多有 si 件，每件体积是 vi，价值是 wi。
求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

### 输入格式
第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。接下来有 N 行，每行三个整数 vi,wi,si，用空格隔开，分别表示第 i 种物品的体积、价值和数量。

### 数据范围
0<N,V≤100
0<vi,wi,si≤100

_之所以题目叫多重背包问题Ⅰ，是因为本题的数据范围很小，用朴素做法是不会TLE的_

> 代码（朴素版）
```js
#include <iostream>
using namespace std;

const int N = 110;
int f[N][N];
int v[N],w[N],s[N];

int n,m;
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i = 1;i <= n;i++) cin >> v[i] >> w[i] >> s[i];
    for(int i = 1;i <= n;i++)
    {
        for(int j = 1;j <= m;j++)
        {
            //关键在于这一层针对 s[]数组的for循环条件  k中s数组取 且 容量不能过背包容量
            for(int k = 0;k <= s[i] && k * v[i] <= j;k++)
            {
                f[i][j] = max(f[i][j],f[i - 1][j - k * v[i]] + k * w[i]);
            }
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```