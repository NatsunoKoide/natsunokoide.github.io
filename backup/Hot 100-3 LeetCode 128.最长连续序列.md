### 题目
给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
请你设计并实现时间复杂度为 O(n) 的算法解决此问题。

### 示例
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。

输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9

### 解法1：排序+去重+判断间隔数字是否连续 连续++ 不连续使得index回到1

> 代码
```js
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int nowIndex = 1;
        int maxIndex = 1;
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        int n = nums.size();
        if(n < 2) return n;
        for(int i = 1;i < n;i++)
        {
            if(nums[i] == nums[i - 1] + 1)
            {
                nowIndex += 1;
                maxIndex = max(maxIndex,nowIndex);
            }
            else
            {
                nowIndex = 1;
            }
        }
        return maxIndex;
    }
};
```

### 解法2（LeetCode题解）：
```js
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
    //用set主要是可以用count（个人理解）
        unordered_set<int> num_set;
        for (const int& num : nums) {
            num_set.insert(num);
        }

        int longestStreak = 0;
        //因为用了哈希存储 所以就没必要排序去重了
        for (const int& num : num_set) {
            //判断不存在前一个数字 即当前数组为起始值  
           //不存在前一个数就往下进行
            if (!num_set.count(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.count(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = max(longestStreak, currentStreak);
            }
        }

        return longestStreak;           
    }
};

作者：力扣官方题解
链接：https://leetcode.cn/problems/longest-consecutive-sequence/solutions/276931/zui-chang-lian-xu-xu-lie-by-leetcode-solution/
来源：力扣（LeetCode）
```