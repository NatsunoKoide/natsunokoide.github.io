### 题目
有 n个人排队到 1个水龙头处打水，第 i个人装满水桶所需的时间是 ti，请问如何安排他们的打水顺序才能使所有人的等待时间之和最小？

### 输入格式
第一行包含整数 n。
第二行包含 n个整数，其中第 i个整数表示第 i个人装满水桶所花费的时间 ti。

### 输出格式
输出一个整数，表示最小的等待时间之和。

### 数据范围
1≤n≤10e5,
1≤ti≤10e4

### 思路
简单来说就是，让最小的先打（排列不等式）

> 代码
```js
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long LL;

const int N = 100010;
LL res = 0;
int t[N];
int n;

int main()
{
    cin >> n;
    for(int i = 0;i < n;i++) scanf("%d",&t[i]);
    sort(t,t+n);
    for(int i = 0;i < n;i++) res += (t[i] * (n - i -1));
    cout << res << endl;
    return 0;
}
```