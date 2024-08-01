### 思路 
**通过对提议的分析 将所需要用到的数字与其对应的罗马数字 制作成pair
   通过不断的最大化减法 累计roma字符 最终获得最优解**

### 注意点
尝试在类定义内部使用初始化列表直接初始化一个非const且非static的数组成员
方法：
1.使用固定大小的数组并直接指定大小：
```js
class MyClass {
public:
    MyClass() : myArray{1, 2, 3} {} // 使用构造函数初始化列表
private:
    int myArray[3]; // 固定大小的数组
};
```
2.在类体外初始化数组成员：
```js
class MyClass {
public:
    static std::pair<int, std::string> pairs[];
};

// 在类体外初始化数组
std::pair<int, std::string> MyClass::pairs[] = {
    {1000, "M"},
    {900,  "CM"},
    {500,  "D"},
    {400,  "CD"},
    // ...
};
```
3.使用构造函数和初始化列表：
```js
class MyClass {
public:
    MyClass() : pairs{ {1000, "M"}, {900, "CM"}, {500, "D"}, {400, "CD"}, /* ... */ } {}
private:
    std::pair<int, std::string> pairs[13]; // 固定大小的数组
};
```

### 代码
```js
const pair<int, string> pairs[] = {
        {1000, "M"},
        {900,  "CM"},
        {500,  "D"},
        {400,  "CD"},
        {100,  "C"},
        {90,   "XC"},
        {50,   "L"},
        {40,   "XL"},
        {10,   "X"},
        {9,    "IX"},
        {5,    "V"},
        {4,    "IV"},
        {1,    "I"},
    };
class Solution {
public:
    const pair<int, string> pairs[] = {
        {1000, "M"},
        {900,  "CM"},
        {500,  "D"},
        {400,  "CD"},
        {100,  "C"},
        {90,   "XC"},
        {50,   "L"},
        {40,   "XL"},
        {10,   "X"},
        {9,    "IX"},
        {5,    "V"},
        {4,    "IV"},
        {1,    "I"},
    };
    string intToRoman(int num) {
        string roma;
        for(const auto &[value,symbol] : pairs)
        {
            while(num >= value)
            {
                num -= value;
                roma += symbol;
            }
            if(num == 0) break;
        }
        return roma;
    }
};
```
