### 题目
**给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
   字母异位词 是由重新排列源单词的所有字母得到的一个新单词。**

### 示例
输入：strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出：[["bat"],["nat","tan"],["ate","eat","tea"]]

_解释：把构成一样的string放在同一个vector中然后分组输出出来_

###  解法：将string提取出来作为key（需要sort），通过key去模式识别动态创建vector存储相同组成的string

> 代码
```js
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for (string &str: strs) {
            string key = str;
            //key从a到z排序
            sort(key.begin(), key.end());
            mp[key].emplace_back(str);
        }
        vector<vector<string>> ans;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            ans.emplace_back(it->second);
        }
        return ans;
    }
};
```
_此段代码使用了引用，当使用引用时，str 实际上指向了 strs 中的元素，而不是对元素的拷贝。这意味着在对 str 进行操作时，会直接影响到 strs 中对应的元素。这可以提高程序的运行效率，因为不需要对元素进行拷贝。这样的使用场景适合对元素的值进行修改的情况。
若不是用引用，那么在每次迭代中都会对 strs 中的元素进行拷贝，然后将拷贝值赋给 str。这样做会占用更多的内存，特别是在 strs 数组较大的情况下。这样的使用场景适合对元素进行只读操作或者在对元素的修改不需要影响到原始数组的情况。_