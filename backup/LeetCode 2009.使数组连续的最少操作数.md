### 题目
给你一个整数数组 nums 。每一次操作中，你可以将 nums 中 任意 一个元素替换成 任意 整数。
如果 nums 满足以下条件，那么它是 连续的 ：
nums 中所有元素都是 互不相同 的。
nums 中 最大 元素与 最小 元素的差等于 nums.length - 1 。
比方说，nums = [4, 2, 5, 3] 是 连续的 ，但是 nums = [1, 2, 3, 5, 6] 不是连续的 。
请你返回使 nums 连续 的 最少 操作次数。

### 示例
示例 1：
输入：nums = [4,2,5,3]
输出：0
解释：nums 已经是连续的了。
示例 2：
输入：nums = [1,2,3,5,6]
输出：1
解释：一个可能的解是将最后一个元素变为 4 。
结果数组为 [1,2,3,5,4] ，是连续数组。
示例 3：
输入：nums = [1,10,100,1000]
输出：3
解释：一个可能的解是：
- 将第二个元素变为 2 。
- 将第三个元素变为 3 。
- 将第四个元素变为 4 。
结果数组为 [1,2,3,4] ，是连续数组。

> 代码
```js
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int n = nums.size();
        //利用set的性质对nums去重
        unordered_set<int> cnt(nums.begin(),nums.end());
        //把去重后的内容存入到vector容器中
        vector<int> sortedUniqueNums(cnt.begin(), cnt.end());
        //排序去重后的数组
        sort(sortedUniqueNums.begin(), sortedUniqueNums.end());
        int res = n,j = 0;//答案最大就是n（全部修改），j是滑动窗口的范围
        //遍历处理过后的数组
        for(int i = 0;i < sortedUniqueNums.size();i++)
        {
            //题目中定义了最大和最小的插值
            int right = sortedUniqueNums[i] + n - 1;
            while(j < sortedUniqueNums.size() && sortedUniqueNums[j] <= right)
            {
                //n-(j-i+1)理解为n里减去满足左右区间条件的数 剩下的 就是要修改的数
                res = min(res,n - (j - i + 1));j++;
            }
        }
        return res;
    }
};
```