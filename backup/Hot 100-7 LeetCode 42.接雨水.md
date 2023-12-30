### 题目
**给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。**
### 示例

示例1：
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/23299e9d-56eb-4741-8efb-ed5856b3fd5c)
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
示例 2
输入：height = [4,2,0,3,2,5]
输出：9

### 方法1—动态规划  时间复杂度O(n)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/bb3e5868-0830-4481-a87d-54e40d75c0cf)
_这个方法是最开始学习接雨水问题时编写得代码，通过空间覆盖计算得思想，这种接雨水的计算方式需记忆。_

> 代码

```js
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) {
            return 0;
        }
        
        vector<int> leftMax(n);
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }

        vector<int> rightMax(n);
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += min(leftMax[i], rightMax[i]) - height[i];
        }
        return ans;
    }
};
```

### 方法2—双指针优化动态规划
_双指针把方法1的时间复杂度优化到了 O(1),整体思想与方法1类似，靠左右指针向内收进行计算，指针重叠即结束。_
```js
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int left = 0, right = height.size() - 1;
        int leftMax = 0, rightMax = 0;
        while (left < right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (height[left] < height[right]) {
                ans += leftMax - height[left];
                ++left;
            } else {
                ans += rightMax - height[right];
                --right;
            }
        }
        return ans;
    }
};
```