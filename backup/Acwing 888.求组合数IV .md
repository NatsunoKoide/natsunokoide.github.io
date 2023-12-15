
### 求组合数的基础公式：
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/d3b140ef-012e-46ab-9edd-974cc4d97757)
### 其中阶乘可以转化为：
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/ec00e7e3-2f42-46a3-bbf5-15df59e956fa)

```js
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 5010;

int primes[N],cnt;
int sum[N];
bool st[N];

void get_primes(int n)   //线性筛法求质数 cnt是个数 primes数组里装着质数
{
    for(int i = 2;i <= n;i++)
    {
        if(!st[i]) primes[cnt++] = i;
        for(int j = 0;primes[j] * i <= n;j++)
        {
            st[primes[j] * i] = true;
            if(i % primes[j] == 0) break;
        }
    }
}

int get(int n,int p) //// 求n！中包含p的次数
{
    int res = 0;
    while(n)
    {
        res += n/p;
        n /= p;
    }
    return res;
}

vector<int> mul(vector<int>& A, int b)   //高精度乘法 用vector接收 和 输入 c++才要
{
    vector<int> C;
    int t = 0;
    for(int i = 0;t || i < A.size();i++)
    {
        if(i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main()
{
    int a,b;
    cin >> a >> b;
    get_primes(a);
    //循环得到每一个质数的次数（这个次数不能重复）
    for(int i = 0;i < cnt;i++) //cnt在get_primes函数里
    {
        int p = primes[i];
        sum[i] = get(a,p) - get(a-b,p) - get(b,p);
    }
    
    vector<int> res;
    res.push_back(1);
    
    for(int i = 0;i < cnt;i++) //循环质数的个数
    {
        for(int j = 0;j < sum[i];j++)//循环质数的次数
        {
            res = mul(res,primes[i]); //算出每一个质数的j次方
        }
    }
    //因为高精度乘法最后的输出是每一个数字以vector的方式呈现的 所以需要遍历整个数组才能得到最后的数字
    for(int i = res.size() - 1;i >= 0;i--) printf("%d",res[i]);
    return 0;
} 
```