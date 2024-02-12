### 题目
给定 n
 堆石子，两位玩家轮流操作，每次操作可以取走其中的一堆石子，然后放入两堆规模更小的石子（新堆规模可以为 0，且两个新堆的石子总数可以大于取走的那堆石子数），最后无法进行操作的人视为失败。
问如果两人都采用最优策略，先手是否必胜。

### 输入格式
第一行包含整数 n。
第二行包含 n 个整数，其中第 i 个整数表示第 i 堆石子的数量 ai。

### 输出格式
如果先手方必胜，则输出 Yes。
否则，输出 No。

### 数据范围
1≤n,ai≤100

### Nim思路
1.题意在于:每一堆可以变成小于原来那堆的任意大小的两堆
2.即a[i]可以拆分成 (b[i],b[j]) ,为了避免重复规定b[i]>=b[j],即：a[i]>b[i]>=b[j]
相当于一个局面拆分成了两个局面，由SG函数理论，多个独立局面的SG值，等于这些局面SG值的异或和。
3.因此需要存储的状态就是sg(b[i])^sg(b[j])
4.其余的内涵操作 包括sg和mex操作以及最后答案的获取可以参考**Acwing891-893**

> 代码
```js
#include <iostream>
#include <cstring>
#include <unordered_set>
using namespace std;

const int N = 110;

int n;
int f[N];
unordered_set<int> S;

int sg(int x)
{
    if(f[x] != -1) return f[x];
    for(int i = 0;i < x;i++)
    {
        for(int j = 0;j <= i;j++)
        {
            S.insert(sg(i) ^ sg(j));
            //相当于一个局面拆分成了两个局面，由SG函数理论，多个独立局面的SG值，等于这些局面SG值的异或和
        }
    }
    //对全部局面做mex操作 即返回 堆x 在计算sg值时第一个没出现的自然数
    for(int i = 0;;i++)
    {
        if(!S.count(i)) return f[x] = i;
    }
}

int main()
{
    memset(f,-1,sizeof f);
    cin >> n;
    int res = 0;
    while (n--)
    {
        int x;
        cin >> x;
        //res是对所有sg（x）做异或操作
        res ^= sg(x);
    }
    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```