### 题目
一个整数数组 original 可以转变成一个 双倍 数组 changed ，转变方式为将 original 中每个元素 值乘以 2 加入数组中，然后将所有元素 随机打乱 。

给你一个数组 changed ，如果 change 是 双倍 数组，那么请你返回 original数组，否则请返回空数组。original 的元素可以以 任意 顺序返回。

### 示例
示例 1：

输入：changed = [1,3,4,2,6,8]
输出：[1,3,4]
解释：一个可能的 original 数组为 [1,3,4] :
- 将 1 乘以 2 ，得到 1 * 2 = 2 。
- 将 3 乘以 2 ，得到 3 * 2 = 6 。
- 将 4 乘以 2 ，得到 4 * 2 = 8 。
其他可能的原数组方案为 [4,3,1] 或者 [3,1,4] 。
示例 2：

输入：changed = [6,3,0,1]
输出：[]
解释：changed 不是一个双倍数组。
示例 3：

输入：changed = [1]
输出：[]
解释：changed 不是一个双倍数组。

> 代码
```js
class Solution {
public:
    vector<int> findOriginalArray(vector<int>& changed) {
        sort(changed.begin(), changed.end());
        unordered_map<int,int> count;
        //排序统计频率的做法 
        for(int a : changed) count[a]++;
        vector<int> res;
        for(int a : changed)
        {
            if(count[a] == 0) continue;
            count[a]--;
            if(count[a * 2] == 0) return {};
            count[a * 2]--;
            //其实就算当count[a]和count[a * 2]同时存在的时候 把a放到答案数组
            res.push_back(a);
        }
        return res;
    }
};
```