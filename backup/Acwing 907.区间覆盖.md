### 题目
给定 N个闭区间 [ai,bi]以及一个线段区间 [s,t]，请你选择尽量少的区间，将指定线段区间完全覆盖。
输出最少区间数，如果无法完全覆盖则输出 −1。

### 输入格式
第一行包含两个整数 s 和 t，表示给定线段区间的两个端点。
第二行包含整数 N，表示给定区间数。
接下来 N 行，每行包含两个整数 ai,bi，表示一个区间的两个端点。

### 输出格式
输出一个整数，表示所需最少区间数。
如果无解，则输出 −1。

### 数据范围
1 <= N <= 1e5

### 分析思路

1. 对区间结构按左端排序
2. 从前往后一次枚举每个区间：判断左端点在st之前的区间，循环找到最大右端点，如果右端点也在st之前，说明无法覆盖。
3. 如果找到左端点在st之前，右端点在st之后的区间，(i++)
4. 每一次循环都使用j遍历之后的区间没满足一次j < n && R[j].l <= st 就扩大一次这个区间 因为题目要求是输出最小的区间数
5. 把start更新成right，保证后面的区间适合之前的区间有交集，从而形成对整个序列的覆盖
6. 每一次循环最后把r值赋给st，下一次循环r又会从负无穷开始
7. 如果遍历了所有的数组，还是没有覆盖最后的end，说明不能成功，如果最后超过了ed证明已经遍历结束 succ置为true 可以输出res

> 代码
```js
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

struct Range
{
    int l,r;
    bool operator < (const Range &w)const
    {
        return l < w.l;
    }
}range[N];

int main()
{
    int st,ed;
    cin >> st >> ed;
    int n;
    cin >> n;
    for(int i = 0;i < n;++i)
    {
        int x,y;
        cin >> x >> y;
        range[i] = {x,y};
    }
    
    sort(range,range + n);
    
    int res = 0;
    bool succ = false;
    
    for(int i = 0;i < n;i++)
    {
        int j = i,r = -2e9;
        while(j < n && range[j].l <= st)
        {
            r = max(r,range[j].r);
            j++;
        }
        if(r < st) break;
        res ++;
        //到底了succ为true表示res确定
        if(r >= ed)
        {
            succ = true;
            break;
        }
        //每一次循环最后把r值赋给st，下一次循环r又会从负无穷开始
        st = r;
        //因为循环完了i会++，这里-1是调整i指向的位置
        i = j - 1;
    }
    if(!succ) res = -1;
    cout << res << endl;
    return 0;
}
```