### 题目 
给定 N个闭区间 [ai,bi]，请你将这些区间分成若干组，使得每组内部的区间两两之间（包括端点）没有交集，并使得组数尽可能小。
输出最小组数。

### 输入格式
第一行包含整数 N，表示区间数。
接下来 N 行，每行包含两个整数 ai,bi，表示一个区间的两个端点。

### 输出格式
输出一个整数，表示最小组数。

### 数据范围
1≤N≤10e5
−10e9≤ai≤bi≤10e9

### 思路分析
1.如果一个区间的左端点比最小组的右端点要小，ranges[i].l<=heap.top() ， 就开一个新组 heap.push(range[i].r);
2.如果一个区间的左端点比最小组的右端点要大，则放在该组， heap.pop(), heap.push(range[i].r);
3. heap.pop() 相当于 把默认融入的组去除掉 把最新的右端值放在优先级队列的头部
4. 每组去除右端点最小的区间，只保留一个右端点较大的区间，这样heap有多少区间，就有多少组。

> 代码
```js
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

const int N = 100010;
int n;
struct Range
{
    int l,r;
    //区间左端点比大小
    bool operator < (const Range &w)const
    {
        return l < w.l;
    }
}range[N];

int main()
{
    scanf("%d",&n);
    for(int i = 0;i < n;i++)
    {
        int l,r;
        scanf("%d%d",&l,&r);
        range[i] = {l,r};
    }
    sort(range,range + n);
    priority_queue<int,vector<int>,greater<int>> heap;
    for(int i = 0;i < n;i++)
    {
        //发现有重叠的区间就开新组
        if(heap.empty() || heap.top() >= range[i].l) heap.push(range[i].r);
        else
        {
            //没发现重叠就去掉最小的范围 把新范围加进去
            heap.pop();
            heap.push(range[i].r);
        }
    }
    printf("%d\n",heap.size());
    return 0;
}
```