### 题目
给你一个字符串 s，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 最后一个 单词的长度。

单词 是指仅由字母组成、不包含任何空格字符的最大
子字符串。

### 知识点
**end（）函数返回的是迭代器指向的元素的下一个元素 所以 在代码中 对其进行 -1 操作 指向最后一个元素**

### 代码
```js
class Solution {
public:
    int lengthOfLastWord(string s) {
        //end（）指向的是最后一个元素的下一个位置
        for (auto it = s.end() - 1 ; it != s.begin(); --it)
        {
            if (*it == ' ')
            {
                s.erase(it, s.end());
            } 
            else 
            {
                 break; 
            }       
        }        
        int n = s.length();
        int res = 0;
        for(int i = 0;i < n;i++)
        {
            if(s[i] != ' ') res++;
            else if (s[i] == ' ')
            {
                res = 0;
            }
        }
        return res;
    }
};
```
 