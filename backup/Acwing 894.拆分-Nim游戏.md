### Nim游戏的基础理论 在 #4 Nim游戏 中
### 拆分-Nim游戏 的 思考方式类似于集合-Nim游戏 需要针对不同的局面进行异或 #7 （SG函数）

> 拆分-Nim游戏的核心
**本题主要在于 每一次拆分两组的数量虽然可以大于原石头堆 但是每一组不可以大于原堆的石头数
   所以每一轮拆分都会至少使得上一轮的最大值-1，从而遍历所有情况总能使得有一方石头堆归零，无法继续操作。**

### 拆分-Nim游戏题目
**给定 n堆石子，两位玩家轮流操作，每次操作可以取走其中的一堆石子，然后放入两堆规模更小的石子（新堆规模可以为 0，且两个新堆的石子总数可以大于取走的那堆石子数），最后无法进行操作的人视为失败。
问如果两人都采用最优策略，先手是否必胜。**

> 代码
```js
#include <iostream>
#include <unordered_set>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 110;
int res = 0;
int f[N];
int n;
unordered_set<int> S;

int sg(int x)
{
.//遍历完所有情况 就会f里就没-1了就返回f数组
    if(f[x] != -1) return f[x];
//拆分之后遍历i，j组
    for(int i = 0;i < x;i++)
    {
        for(int j = 0;j <= i;j++)
        {
//sg(i,j) = sg(i) ^ sg(j)
            S.insert(sg(i) ^ sg(j));
        }
    }
// 上方的循环会递归到最后，从最后一个数开始向上返回
    for(int i = 0;;i++)
    {
        if(!S.count(i))
        {
            return f[x] = i;
        }
    }
}

int main()
{
    memset(f,-1,sizeof f);
    cin >> n;
    while(n--)
    {
        int x; 
        cin >> x;
        res ^= sg(x);
    }
    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```