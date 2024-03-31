### 题目
序列化二叉树的一种方法是使用 前序遍历 。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/9043d643-826d-46c4-8542-281466f2105f)
例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。
给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。
保证 每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。
你可以认为输入格式总是有效的
例如它永远不会包含两个连续的逗号，比如 "1,,3" 。
注意：不允许重建树。

### 示例
示例 1:
输入: preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#"
输出: true
示例 2:
输入: preorder = "1,#"
输出: false
示例 3:
输入: preorder = "9,#,#,1"
输出: false
 
### 分析
定义一个概念，叫做槽位。一个槽位可以被看作「当前二叉树中正在等待被节点填充」的那些位置。
二叉树的建立也伴随着槽位数量的变化。每当遇到一个节点时：
如果遇到了空节点，则要消耗一个槽位；
如果遇到了非空节点，则除了消耗一个槽位外，还要再补充两个槽位。
此外，还需要将根节点作为特殊情况处理。
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/ce257022-73b9-4042-8685-dc6d143688b5)

> 代码
```js
class Solution {
public:
    bool isValidSerialization(string preorder) {
        int n = preorder.length();
        int i = 0;
        //初始化槽位 1 
        int slots = 1;
        while (i < n) {
            //while循环内槽位归零 为false
            if (slots == 0) {
                return false;
            }
            if (preorder[i] == ',') {
                i++;
            } else if (preorder[i] == '#'){
                slots--;
                i++;
            } else {
                // 读一个数字,可能会出现多位数所以用while
                while (i < n && preorder[i] != ',') {
                    i++;
                }
                //非空节点创造了两个槽位自己扣除一个原有槽位
                slots++; // slots = slots - 1 + 2
            }
        }
        return slots == 0;
    }
};
```