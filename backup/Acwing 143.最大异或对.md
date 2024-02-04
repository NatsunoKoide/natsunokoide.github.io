### 题目
在给定的 N个整数 A1，A2……AN中选出两个进行 xor（异或）运算，得到的结果最大是多少？

### 输入格式
第一行输入一个整数 N。
第二行输入 N个整数 A1～AN。

### 输出格式
输出一个整数表示答案。

### 数据范围
1≤N≤10^5
0≤Ai<2^31

### 优化异或对象的选取
采用trie树进行数字存储，二进制形式判断最优解 ，对树进行剪枝 降低了时间复杂度

> 暴力代码
```js
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010;

int n;
int a[N];

int main()
{
    scanf("%d",&n);
    for(int i = 0;i < n;i++)scanf("%d",&a[i]);
    int res = 0;
    for(int i = 0;i < n; i++)
    {
        for(int j = 0;j < n;j++)
        {
            res = max(res,a[i] ^ a[j]);
        }
    }
    printf("%d\n",res);
    return 0;
}
```

> 优化后的代码（tire树+ 二进制）
```js
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010,M = 31*N;

int n;
int a[N];
int son[M][2],idx;

void insert(int x)
{
    int p=0;   //trie 树的头节点
    for(int i = 30;i >= 0;i--)
    {
        int u = x>>i&1;
        if(!son[p][u])son[p][u] = ++idx;
        p=son[p][u];
    }
}

int query(int x)
{
    int p = 0,res = 0;
    for(int i = 30;i >= 0;i--)
    {
        int u = x>>i&1;
        if(son[p][!u])
        {
            p=son[p][!u];
            res = res * 2 + !u ;//二进制的计算方式 即在左右一位补一个0或者1
        }
        else
        {
            p=son[p][u];
            res = res * 2 +u;
        }
    }
    return res;
}

int main()
{
    scanf("%d",&n);
    for(int i = 0;i < n;i++)scanf("%d",&a[i]);
    int res = 0;
    for(int i = 0;i < n; i++)
    {
        insert(a[i]);
        res = max(res,a[i]^query(a[i]));
    }
    printf("%d\n",res);
    return 0;
}
```