### 题目
一个正整数 n 可以表示成若干个正整数之和，形如：n=n1+n2+…+nk，其中 n1≥n2≥…≥nk,k≥1。
我们将这样的一种表示称为正整数 n 的一种划分。
现在给定一个正整数 n，请你求出 n 共有多少种不同的划分方法。

### 输入格式
共一行，包含一个整数 n。

### 输出格式
共一行，包含一个整数，表示总划分数量。
由于答案可能很大，输出结果请对 1e9+7取模。

### 数据范围
1≤n≤1000

### 思路解析

1. 题目转化为dp问题：把1,2,3, … n分别看做n个物体的体积，这n个物体均无使用次数限制，问恰好能装满总体积为n的背包的总方案数（完全背包问题变形）
2. 数组初始化：求最大值时，当都不选时，价值显然是 0
    而求方案数时，当都不选时，方案数是 1（即前 i 个物品都不选的情况也是一种方 
    案），所以需要初始化为 1
    即：for (int i = 0; i <= n; i ++) f[i][0] = 1;
    等价变形后： f[0] = 1
3. 状态数组 f[i][j]：前i个整数（1,2…,i）恰好拼成j的方案数 ！！！！！！！！！！！
4. 求解方案数：f[i][j] = f[i - 1][j] + f[i - 1][j - i] + f[i - 1][j - 2 * i] + ...;
                          f[i][j - i] = f[i - 1][j - i] + f[i - 1][j - 2 * i] + ...; 
                          f[i][j]=f[i−1][j]+f[i][j−i]  （类似于完全背包的推导）
5.代码层面的问题：状态分为两种情况：
                         （1）不选第i个物品：f[i][j] = f[i - 1][j] % mod
                         （2）选第i个物品，并循环选几个第i个物品（当容量大于物品大小的时候）： if (j >= i) f[i][j] = (f[i - 1][j] + f[i][j - i]) % mod; 

> 完全背包解法 O（n2）
```js
//状态转移方程
//f[i][j] = f[i - 1][j] + f[i][j - i]
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 1e3 + 10,mod = 1e9 + 7;
int f[N][N];

int main() {
    int n;
    cin >> n;

    for (int i = 0; i <= n; i ++) {
        f[i][0] = 1; // 容量为0时，前 i 个物品全不选也是一种方案
    }

    for (int i = 1; i <= n; i ++) {
        for (int j = 0; j <= n; j ++) {
            f[i][j] = f[i - 1][j] % mod; // 特殊 f[0][0] = 1
            if (j >= i) f[i][j] = (f[i - 1][j] + f[i][j - i]) % mod;
        }
    }
    cout << f[n][n] << endl;
    return 0;
}
```

> 一维优化完全背包代码

> 替换：f[i][j] = f[i-1][j] + f[i][j-1]
   优化一维：f[j] = f[j] + f[j-i]
```js
#include <iostream>
using namespace std;

const int N = 1e3 + 10,mod = 1e9 + 7;
int f[N];
int main()
{
    int n;
    cin >> n;
    f[0] = 1; 
    //容量为0 前i个物品全不选也是一种方案
    //等价之后的一维dp代码
    for(int i = 1;i <= n;i++)
    {
        for(int j = i;j <= n;j++)
        {
            f[j] = (f[j] + f[j - i]) % mod;
        }
    }
    cout << f[n] <<endl;
    return 0;
}
```