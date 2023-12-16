### 卡特兰数
    卡特兰数是组合数学中一个常出现于各种计数问题中的数列。
### 卡特兰数的几何意义
    卡特兰数就是一个有规律的数列，在坐标图中可以表示为：从原点(0,0)出发，每次向x轴或者y轴正方向移动1个单位，直到到达(n,n)点，且在移动过程中不越过第一象限平分线的移动方案总数。
### 卡特兰数的推导以及图解
图解：
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/1cbe6346-e96c-4a49-a8c2-f6e17cd5cd39)
推导过程：
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/1a8c5bd2-ca52-4c6e-ae89-9ed78bf564f4)

### 卡特兰数的应用 （Acwing 899.满足条件的01序列）
    该题主要解决给定 n个 0和 n个 1，它们将按照某种顺序排成长度为 2n的序列，求它们能排列成的所有序列中，能够满足任意前缀序列中 0 的个数都不少于 1的个数的序列有多少个。 （其本质最优解就是卡特兰数）
```js
#include <iostream>
#include <algorithm>
using namespace std;

typedef long long LL;

const int mod = 1e9 + 7;

//快速幂  本题目用于求 逆元  费马定理 qmi(i,mod - 2,mod)   
//mod - 2 次方 % mod 为 i 的逆元
int qmi(int a,int k,int p)
{
    int res = 1;
    while(k)
    {
        if(k & 1) res = (LL) res * a % mod;
        k >>= 1;
        a = (LL)a * a % p;
    }
    return res;
}

int main()
{
    int n;
    cin >> n;
    int a = 2 * n,b = n;
    int res = 1;
    //分子（分子的a！与分母的（a-b）!抵消 得到 a*（a-1）*（a-2）...*(a-b+1)）
    for(int i = a;i > a - b;i--) res = (LL)res * i % mod;
    //分母（b!）
    for(int i = 1;i <= b;i++) res = (LL)res * qmi(i,mod - 2,mod) % mod;
    //最后还要乘以一个n+1的逆元
    res = (LL)res * qmi(n + 1,mod - 2,mod) % mod;
    cout << res << endl;
    return 0;
}
```