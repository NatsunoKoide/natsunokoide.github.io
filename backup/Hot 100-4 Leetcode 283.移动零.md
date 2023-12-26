### 题目
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
请注意 ，必须在不复制数组的情况下原地对数组进行操作。

### 示例
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
输入: nums = [0]
输出: [0]

_因为是对数组内部按条件位移，考虑采用双指针进行_


> 代码
```js
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size(),left = 0,right = 0;
        while(right < n)
        {
            if(nums[right] != 0)
            {
                swap(nums[left],nums[right]);
                left++;
            }
            right++;
        }
    }
};
```
**上述代码含义：如果数组没有0，那么快慢指针始终指向同一个位置，每个位置自己和自己交换；如果数组有0，快指针先走一步，此时慢指针对应的就是0，所以要交换。**
