### 题目
给你一个下标从 0 开始的字符串 word ，长度为 n ，由从 0 到 9 的数字组成。另给你一个正整数 m 。
word 的 可整除数组 div  是一个长度为 n 的整数数组，并满足：
1.如果 word[0,...,i] 所表示的 数值 能被 m 整除，div[i] = 1
2.否则，div[i] = 0
返回 word 的可整除数组。

### 示例
输入：word = "998244353", m = 3
输出：[1,1,0,0,0,1,1,0,0]
解释：仅有 4 个前缀可以被 3 整除："9"、"99"、"998244" 和 "9982443" 。
输入：word = "1010", m = 10
输出：[0,1,0,1]
解释：仅有 2 个前缀可以被 10 整除："10" 和 "1010" 。

### 分析
1.此题数据范围过大 需要用longlong管理由string 转换 而来的数
2.string字符串转换 需要 对字符 -‘0’ 才能得到需要的数

> 代码
```js
class Solution {
public:
    vector<int> divisibilityArray(string word, int m) {
        vector<int> div;
        long long num = 0;
        for(int i = 0;i < word.size();i++)
        {
            num = (num * 10 + (word[i] - '0')) % m;
            if(num == 0) div.push_back(1);
            else div.push_back(0);
        }
        return div;
    }
};
```