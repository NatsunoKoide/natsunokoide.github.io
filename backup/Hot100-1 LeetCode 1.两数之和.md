### 题目
**给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。你可以按任意顺序返回答案。**

### 方法一 暴力枚举

> 代码
```js
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0;i < nums.size();i++)
        {
            for(int j = i + 1;j < nums.size();j++)
            {
                if(nums[i] + nums[j] == target) 
                {
                    return {i,j};
                }
            }
        }
        return {};
    }
};
```

### 方法二 哈希查询

> 代码
```js
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> map;
        for(int i = 0;i < nums.size();i++)
        {
            auto iter = map.find(target - nums[i]);
            if(iter != map.end()) return {iter->second,i};
            map.insert(pair<int,int>(nums[i],i));
        }
        return {};
    }
};
```