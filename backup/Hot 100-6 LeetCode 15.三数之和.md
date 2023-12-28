### 题目
**给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请你返回所有和为 0 且不重复的三元组。
注意：答案中不可以包含重复的三元组。**

### 示例
示例 1：
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。

示例 2：
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
示例 3：

输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。

_通过三个指针来分别代表三个数组的指向，最后用vector<vector<int>>存储最后的答案_
_sort操作的原因：
1.去重：排序后，重复的元素会相邻。通过比较相邻元素的值，可以跳过重复的元素，避免产生重复的结果。
2.确定三元组的范围：在内层循环中，将首尾指针向内移动来寻找满足条件的三元组。如果数组未排序，则无法确定指针的移动方向，无法正确寻找满足条件的三元组。
每一层循环最开始都判断了相邻两个数是否一样，如果一样就不需要进入后面的流程减少计算量_

> 代码
```js
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        int n = nums.size();
        sort(nums.begin(),nums.end());
        for(int first = 0;first < n;first++)
        {
            if(first > 0 && nums[first] == nums[first - 1]) continue;
            int third = n - 1;
            int target = -nums[first];
            for(int second = first + 1;second < n;second++)
            {
                if(second > first + 1 && nums[second] == nums[second - 1])continue;
                while(second < third && nums[second] + nums[third] > target) third--;
                if(second == third) break;
                else if(nums[second] + nums[third] == target)
                {
                    res.push_back({nums[first],nums[second],nums[third]});
                }
            }
        }
        return res;
    }
};
```