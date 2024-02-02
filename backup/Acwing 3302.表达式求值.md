### 题目
给定一个表达式，其中运算符仅包含 +,-,*,/（加 减 乘 整除），可能包含括号，请你求出表达式的最终值。
注意：
数据保证给定的表达式合法。
题目保证符号 - 只作为减号出现，不会作为负号出现，例如，-1+2,(2+2)*(-(1+1)+2) 之类表达式均不会出现。
题目保证表达式中所有数字均为正整数。
题目保证表达式在中间计算过程以及结果中，均不超过 231−1。
题目中的整除是指向 0 取整，也就是说对于大于 0 的结果向下取整，例如 5/3=1，对于小于 0 的结果向上取整，例如 5/(1−4)=−1。
C++和Java中的整除默认是向零取整；Python中的整除//默认向下取整，因此Python的eval()函数中的整除也是向下取整，在本题中不能直接使用。

### 输入格式
共一行，为给定表达式。

### 输出格式
共一行，为表达式的结果。

### 模拟
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/67ad530a-b6d3-437f-8ea1-9e75bfebaa04)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/4b33ebbd-15e5-4313-81c8-85afc6c15c14)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/b8ce049a-fb0b-4ada-8967-91483a21db24)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/b49bb098-f2d4-4124-bd6b-7222a66016c8)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/7957a084-4e95-4ba2-a790-f94fce7b88d3)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/622a13e5-9347-4ef0-bebb-49f1fa8600cc)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/d3149d9a-791c-449a-bd2b-df48fbf19170)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/8f578521-774b-45c9-be56-ec7f19c51474)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/5359651e-72aa-4ebf-bb63-44e699f084e0)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/7a983fbe-af4f-47ca-8b98-35e0dfb14374)
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/bcffee77-1470-4875-bee6-cb34dd7dbc49)


> 代码
```js
#include <iostream>
#include <stack>
#include <string>
#include <unordered_map>

using namespace std;

stack<int> num;//存数字
stack<char> op;//存符号

//优先级表
unordered_map<char,int> pr = {{'+',1},{'-',1},{'*',2},{'/',2}};

//栈顶两数的计算
void eval()
{
    int b = num.top();num.pop();
    int a = num.top();num.pop();
    char c = op.top();op.pop();
    int x;
    if(c == '+') x = a + b;
    else if(c == '-') x = a - b;
    else if(c == '*') x = a * b;
    else x = a / b;
    num.push(x);
}

int main()
{
    string s;
    cin >> s;
    for(int i = 0;i < s.size();i++)
    {
        char c = s[i];
        if(isdigit(c))
        {
            int x = 0,j = i;
            while(j < s.size() && isdigit(s[j])) x = 10 * x + s[j++] - '0';
            i = j - 1;
            num.push(x);
        }
        else if(c == '(') op.push(c);
        else if(c == ')')
        {
            //等到）要把里面的内容算完直到碰到（
            while(op.size() && op.top() != '(')eval();
            op.pop();
        }
        else
        {
            //后进来的符号如果优先级高就会直接放入op
            //如果优先级低 会先把之前的两个数字加一个符号算完之后再把符号放入op
            while(op.size() && pr[op.top()] >= pr[c]) eval();
            op.push(c);
        }
    }
    while(op.size()) eval();
    cout << num.top() << endl;
    return 0;
}
```