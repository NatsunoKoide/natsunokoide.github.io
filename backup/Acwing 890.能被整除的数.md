### 容斥原理
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/74e4f052-c584-420d-9027-d5efab16034b)

### 实现思路
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/9ec8a3d0-3385-4127-a98a-217d07e992be)

> 此处给出一个答案用例 

**n= 10，m = 2 ，p1 = 2， p2 = 3 解释为求1-10中能够满足被2或3整除的数字个数，答案为2，3，4，6，8，9，10，共7个**

### Acwing 890.能被整除的数 代码 + 解析
```js
#include <iostream>
using namespace std;

using LL = long long;

const int N = 20;
int p[N],n,m;
int res = 0;

int main()
{
    cin >> n >> m;
    //先把输入的质数存到p数组中
    for(int i = 0;i < m;i++) cin >> p[i];
    // 1 << m 代表的是2的m次方 也就是2的m次方个二进制情况
    for(int i = 1;i < 1 << m;i++)
    {
        int t = 1;//用于记录当前组质数的总乘积
        int s = 0; //用来记录这一组二进制数有几个1，几个1代表包含几个集合
        //遍历当前组的各个位置
        for(int j = 0;j < m;j++)
        {
            //判断到当前位置为1
            if(i >> j & 1)
            {
                //质数的总乘积如果已经大于n呢么就break，此集合是空集
                if((LL)t * p[j] > n) 
                {
                    t = -1;
                    break;
                }
                s++; //i >> j & 1 判定到当前位是1 s++
                t *= p[j]; //把当前1位置的质数乘到t中
            }
        }
        if(t != -1) //如果t的值没有超出总数，就对res进行计算
        {
            //这里涉及到容斥原理中 计算集合个数的公式 Si = n/pi（向下取整）（n为总数，pi为质数）
            //之前用t存储当前组所有质数的乘积 因为 S1交S2 = n/（p1*p2），所以使用t的乘积可以多个集合的情况
            //s代表1的个数，这是因为容斥原理每一项的系数为（-1）的（n-1）次方（n为1的个数）
            if(s & 1) res += n / t; //(s & 1)若为1 证明当前组包含奇数个集合（奇数个1）
            //如果是奇数的组合 根据容斥原理 为+号
            else res -= n / t; //反之为-号
        }
    }
    cout << res << endl;
    return 0;
}
```

_此题涉及到容斥原理 和 二进制处理集合的情况_