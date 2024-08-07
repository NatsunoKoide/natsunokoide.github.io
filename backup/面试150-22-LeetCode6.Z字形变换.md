### 题目
将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

P   A   H   N
A P L S I I G
Y   I   R
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
 

示例 1：

输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
示例 2：
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
示例 3：

输入：s = "A", numRows = 1
输出："A"


### 思路
模拟这个行索引的变化，在遍历 s 中把每个字符填到正确的行 res[i] 。
按顺序遍历字符串 s ：
res[i] += c： 把每个字符 c 填入对应行 s
i += flag： 更新当前字符 c 对应的行索引；
flag = - flag： 在达到 Z 字形转折点时，执行反向。

### 代码
```js
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1) return s;
        vector<string> rows(numRows);
        int i = 0;
        int flag = -1;
        for(char str : s)
        {
            rows[i].push_back(str);
            if(i == 0 || i == numRows - 1)
            {
                flag = -flag;
            }
            i += flag;
        }
        string res;
        for(auto row : rows)
        {
            res += row;
        }
        return res;
    }
};
// string由char组成
// 用for ： 提取字符串 用i和flag控制容器添加的走向
```