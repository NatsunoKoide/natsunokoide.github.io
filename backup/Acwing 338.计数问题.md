### 题目
给定两个整数 a 和 b，求 a 和 b 之间的所有数字中 0∼9的出现次数。
例如，a=1024，b=1032，则 a和 b之间共有 9个数如下：
1024 1025 1026 1027 1028 1029 1030 1031 1032
其中 0 出现 10 次，1 出现 10次，2 出现 7次，3 出现 3次等等…

### 输入格式
输入包含多组测试数据。
每组测试数据占一行，包含两个整数 a和 b。
当读入一行为 0 0 时，表示输入终止，且该行不作处理。

### 输出格式
每组数据输出一个结果，每个结果占一行。
每个结果包含十个用空格隔开的数字，第一个数字表示 0 出现的次数，第二个数字表示 1 出现的次数，以此类推。

### 数据范围
0<a,b<100000000

### ycx基础课 
```js
//yxc算法基础课写法
/*
针对1~n中计算目标x的个数进行分类讨论

001~abc-1, 999

abc
    1. num[i] < x, 0
    2. num[i] == x, 0~efg
    3. num[i] > x, 0~999

*/

#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

//获取一个数字的一段区间
int get(vector<int> num,int l,int r)
{
    int res = 0;
    for(int i = l;i >= r;i--) res = res * 10 + num[i];
    return res;
}

//自己写一个指数运算
int power10(int i)
{
    int res = 1;
    while(i--) res *= 10;
    return res;
}

//计算0~n中计算目标x的个数
int count(int n,int x)
{
    if(!n) return 0；
    //用num存储每一位数字
    vector<int> num; 
    while(n)
    {
        //从低位开始存储数字
        num.push_back(n % 10);
        n /= 10;
    }
    //得到数字的长度
    n = num.size();
    int res = 0;
    
    //动态规划 分类讨论
    //！x出现的原因是不能出现前导零，从第二位开始枚举 排除第一位是0的情况
    
    for(int i = n - 1 - !x;i >= 0;i--)
    {
    //这里需要注意，我们的长度需要减一，是因为num是从0开始存储，而长度是元素的个数，因此需要减1才能读到正确的数值
        //第一分类,000~abc-1,那么此时情况个数就会是abc*10^3，这里的3取决于后面efg的长度，假如他是efgh，那么就是4
        res += get(num,n - 1,i + 1) * power10(i);
        //列举0出现的次数 
        //例如abcdefg我们列举d，那么他就得从001~abc-1，这样就不会直接到efg，而是会到0efg，因为前面不是前导零，自然就可以列举这个时候0出现的次数，所以要减掉1个power10
        if(!x) res -= power10(i);
        //第二分类
        if(num[i] == x) res += get(num,i - 1,0) + 1;
        else if(num[i] > x) res += power10(i);
    }
    return res;
}

int main()
{
    int a,b;
    //循环读入a，b 直到 a或者b中出现0
    while(cin >> a >> b, a || b)
    {
        if(a > b) swap(a,b);
        for(int i = 0;i <= 9;i++)
        {
            cout << count(b,i) - count(a - 1,i) <<' ';
        }
        cout << endl;
    }
    return 0;
}

```