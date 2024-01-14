### 题目
给定一个长度为 N的数列，求数值严格单调递增的子序列的长度最长是多少。

### 输入格式
第一行包含整数 N。
第二行包含 N个整数，表示完整序列。

### 输出格式
输出一个整数，表示最大长度。

### 数据范围
1≤N≤100000，
−10e9≤数列中的数≤10e9

> 当前题目为Acwing 896.最长上升子序列Ⅱ 在Ⅰ的基础上对N 的范围进行了扩大，背包的思考方式存在时间复杂度压力，所以采用模拟单调栈的贪心思路求解。

### 代码一（采用low_bound和容器）
```js
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main()
{
    int n; cin >> n;
    vector<int>arr(n);
    for (int i = 0; i < n; ++i)cin >> arr[i];

    vector<int>stk;//模拟堆栈
    stk.push_back(arr[0]);
    
    for (int i = 1; i < n; ++i) {
        if (arr[i] > stk.back())//如果该元素大于栈顶元素,将该元素入栈
            stk.push_back(arr[i]);
        else//替换掉第一个大于或者等于这个数字的那个数
            *lower_bound(stk.begin(), stk.end(), arr[i]) = arr[i];
    }
    cout << stk.size() << endl;
    return 0;
}
```

### 代码二（使用二分查找模拟单调栈的替换位置）

> 代码二的时间复杂度是代码一的四分之一
```js
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010;
//a[]存放数字 f[]模拟单调栈
int a[N],f[N];
//最长子序列的长度
int cnt;

int find(int x)
{
    int l = 1,r = cnt;
    while(l < r)
    {
        int mid = l + r>> 1;
        if(f[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return l;
}

int main()
{
    int n;
    scanf("%d",&n);
    for(int i = 1;i <= n;i++) scanf("%d",&a[i]);
    f[++cnt] = a[1];
    for(int i = 2;i <= n;i++)
    {
        if(a[i] > f[cnt]) f[++cnt] = a[i];
        else 
        {
            //find函数找的是在0-cnt个数字中比a[i]小或者等于的那个数字的位置
            int tmp = find(a[i]);
            f[tmp] = a[i];
        }
    }
    printf("%d",cnt);
    return 0;
}
```