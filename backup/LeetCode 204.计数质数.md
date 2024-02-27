### 题目
给定整数 n ，返回 所有小于非负整数 n 的质数的数量 。

### 示例
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
输入：n = 0
输出：0
输入：n = 1
输出：0

### 分析
1.筛选质数一共三种方法 普通筛选 埃氏筛 线性筛
2.0 和 1 不是质数

> 代码1——埃氏筛
```js
class Solution {
public:
    int countPrimes(int n) {
        vector<int> isPrime(n,1);
        int res = 0;
        for(int i = 2;i < n;i++)
        {
            //如果是质数
            if(isPrime[i])
            {
                //是质数答案+1
                res += 1;
                //埃氏筛要防止i*i越界
                if((long long) i * i < n)
                {
                    //利用质数去把后面所有跟这个质数有关的数全部判false/0
                    for(int j = i * i;j < n;j += i)
                    {
                        isPrime[j] = 0;
                    }
                }
                
            }
        }
        return res;
    }
};
```

> 代码1 把1/0换成 bool类型 执行速度慢了一倍 内存减少十倍

```js
class Solution {
public:
    int countPrimes(int n) {
        vector<bool> isPrime(n,true);
        int res = 0;
        for(int i = 2;i < n;i++)
        {
            //如果是质数
            if(isPrime[i])
            {
                //是质数答案+1
                res += 1;
                //埃氏筛要防止i*i越界
                if((long long) i * i < n)
                {
                    //利用质数去把后面所有跟这个质数有关的数全部判false/0
                    for(int j = i * i;j < n;j += i)
                    {
                        isPrime[j] = false;
                    }
                }
                
            }
        }
        return res;
    }
};
```

> 代码2——线性筛 非常推荐的一种算法 既可以得到质数是什么也可以得到质数的个数

```js
class Solution {
public:
    int countPrimes(int n) {
        vector<int> primes;
        vector<bool> isPrime(n,true);
        int res = 0;
        for(int i = 2;i < n;i++)
        {
            //如果是质数 就放入primes
            if(isPrime[i]) primes.push_back(i);
            //线性筛 利用 primes数组
            for(int j = 0;primes[j] * i < n;j++)
            {
                isPrime[i * primes[j]] = false;
                if(i % primes[j] == 0) break; 
            }
        }
        return primes.size();
    }
};
```