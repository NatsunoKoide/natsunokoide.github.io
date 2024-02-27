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