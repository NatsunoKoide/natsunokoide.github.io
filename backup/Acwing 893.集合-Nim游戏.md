### 集合-Nim游戏
**集合类型的Nim游戏问题在Nim之前需要对每一个集合进行SG函数计算并分析SG的结果**

> 知识点 1 mex( )函数
_mex（）：设集合S是一个非负整数集合，定义mex（S）为求出不属于S的最小非负整数的运算
  mes（S）= min[x],其中x属于自然数，且x不属于_
**可以理解为mex就是得出S中不存在的最小的数**

> 知识点 2 SG( )函数
_SG( ):：在有向图中，对于每个节点x,设x触发共有k条边，分别到达节点y1，y2……yk
  SG(x)为x的后继节点的SG值构成的集合执行mex（）运算后的值
  即SG（x） = mex（SG(y1),SG(y2),SG(y3)……SG（yk））
性质1：SG(i)  = k，则i最大能到达的SG值为k-1
性质2：非0可以走向0
性质3：0只能走向非0_

### 本题最重要的定理
**对于n个图（集合）如果SG（G1）^SG(G2)^……SG(Gn) != 0 则先手必胜，反之先手必败**

_对于理论部分，引用Acwing用户E.lena的手写图解_
[https://www.acwing.com/solution/content/23435/](url)

### 题目内容
**给定 n堆石子以及一个由 k个不同正整数构成的数字集合 S。
现在有两位玩家轮流操作，每次操作可以从任意一堆石子中拿取石子，每次拿取的石子数量必须包含于集合 S，最后无法进行操作的人视为失败。
问如果两人都采用最优策略，先手是否必胜。**

> 代码
```js
#include <iostream>
#include <cstring>
#include <algorithm>
#include <set>

using namespace std;

const int N = 110, M = 10010;
int res = 0;

int n,m;
int f[M],s[N];///s存储的是可供选择的集合,f存储的是所有可能出现过的情况的sg值

int sg(int x)
{
    //已经存储过了 返回即可
    if(f[x] != -1) return f[x];
    //set代表的是有序集合(注:因为在函数内部定义,所以下一次递归中的S不与本次相同)
    set<int> S;
    //m为数字集合大小
    for(int i = 0;i < m;i++)
    {
        //s[]内表示可选择拿石头的数量标准
        int sum = s[i];
        //延申到终点的sg值后，再从后往前递归出所有sg值
        if(x >= sum)S.insert(sg(x-sum));
    }
    for(int i = 0;;i++)
    {
        //从0开始++ 找出第一个没有出现的自然数
        if(!S.count(i)) return f[x] = i;
    }
}

int main()
{
    cin >> m;
    for(int i = 0;i < m;i++) cin >> s[i];
    cin >> n;
    memset(f,-1,sizeof(f)); //初始化为-1，方便在sg中查看x是否被记录过
    //对n堆进行处理
    for(int i = 0;i < n;i++)
    {
        int x; 
        cin>>x;
        res ^= sg(x);
    }
    if(res) puts("Yes");
    else puts("No");
    return 0;
}
```