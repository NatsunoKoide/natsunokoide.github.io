### 题目
给定 N个闭区间 [ai,bi]，请你在数轴上选择尽量少的点，使得每个区间内至少包含一个选出的点。
输出选择的点的最小数量。
位于区间端点上的点也算作区间内。

### 输入格式
第一行包含整数 N，表示区间数。接下来 N行，每行包含两个整数 ai,bi，表示一个区间的两个端点。

### 输出格式
输出一个整数，表示所需的点的最小数量。

### 思路分析
1.结论：将每个区间按照右端点从小到大进行排序，从前往后枚举区间，end值初始化为无穷小
如果本次区间不能覆盖掉上次区间的右端点， ed < range[i].l
说明需要选择一个新的点， res ++ ; ed = range[i].r;
2.证明：由贪心策略可知，贪心方法将会选择尽量少的点覆盖所有的区间，且每个点对应的区间的集合不相交；使用反证法，不妨假设ans<cnt成立，由于ans是最优解，那么ans个点也覆盖所有的区间，即存在更少的点ans可以覆盖所有的区间。这与贪心策略不符，因为一旦有更少的点可以覆盖所有的区间，那么它一定是贪心解cnt，否则更少的点不能覆盖所有的区间，与假设矛盾。因此ans≥cnt成立。

3.人话：和leetcode中的穿针引线一样，是一种判断区间的问题。

> 利用vector<PII>存储区间代码
```js
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

typedef pair<int,int> PII;

vector<PII> range;
int n;

bool cmp(PII x,PII y)
{
    return x.second < y.second;
}

int main()
{
    cin >> n;
    while(n--)
    {
        PII p;
        cin >> p.first >> p.second;
        range.push_back(p);
    }
    sort(range.begin(),range.end(),cmp);
    int res = 1;
    for(int i = 1,j = 0;i < range.size();i++)
    {
        if(range[i].first > range[j].second)
        {
            res++;
            j = i;
        }
    }
    cout << res << endl;
    return 0;
}
```

> acwing基础课yxc版本
```js
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;
//利用结构体存储区间————知识点1
struct Range
{
    int l, r; 
    //重载<号 
   //第一个const：const& 表示常引用，防止在函数内部对参数值的修改，以确保函数不会意外改变传入的对象。
    //第二个const：用于修饰函数本身，表示成员函数是一个常量成员函数。这意味着该成员函数在内部不会修改成员变量的值。常量成员函数可以在常对象中调用，但不能修改对象的状态。
    bool operator< (const Range &W)const
    {
        return r < W.r;
    }
}range[N];  //创建结构体的同时直接开辟结构体数组

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ ) scanf("%d%d", &range[i].l, &range[i].r);
    //结构体内部已经编译了sort规则，这里sort不用写第三个参数
    sort(range, range + n);
    //最开始需要与最小值比较一次所以res定义为0而不是1
    int res = 0, ed = -2e9; //最开始定义一个无穷小的ed
    for (int i = 0; i < n; i ++ )
        if (ed < range[i].l)
        {
            res ++ ;
            ed = range[i].r;
        }

    printf("%d\n", res);

    return 0;
}
```