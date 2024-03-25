### 题目（322）
给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。
计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。
你可以认为每种硬币的数量是无限的。
示例 1：
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
示例 2：
输入：coins = [2], amount = 3
输出：-1
示例 3：
输入：coins = [1], amount = 0
输出：0

> 代码（动态规划）
```js
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1,INT_MAX);
        //总金额是 0 的时候没组合数
        dp[0] = 0;
        for(int i = 0;i < coins.size();i++)
        {
            for(int j = coins[i];j <= amount;j++)
            {
                //因为对dp数组进行了int最大值初始化
                //需要判断一下是否不是初始值 才能进行dp
                if(dp[j - coins[i]] != INT_MAX)
                    dp[j] = min(dp[j - coins[i]] + 1,dp[j]);
            }
        }
        if(dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```


### 题目（518）
给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。
请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。
假设每一种面额的硬币有无限个。 
题目数据保证结果符合 32 位带符号整数。
示例 1：
输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
示例 2：
输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。
示例 3：
输入：amount = 10, coins = [10] 
输出：1

> 代码（动态规划）
```js
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        //dp[x] 表示金额之和等于 x 的硬币组合数
        vector<int> dp(amount+1,0);
        dp[0] = 1;
        for(int coin : coins)
        {
            for(int i = coin;i <= amount;i++)
            {
                dp[i] += dp[i - coin];
            }
        }
        return dp[amount];
    }
};
```