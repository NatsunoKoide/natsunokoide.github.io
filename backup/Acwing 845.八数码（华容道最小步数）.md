### 题目
在一个 3×3的网格中，1∼8 这 8 个数字和一个 x 恰好不重不漏地分布在这 3×3的网格中。
在游戏过程中，可以把 x 与其上、下、左、右四个方向之一的数字交换（如果存在）。
现在，给你一个初始网格，请你求出得到正确排列至少需要进行多少次交换。
理解为：将 输入 1 2 3 x 4 6 7 5 8 最终变为 1 2 3 4 6 7 5 8 x。

### 输入格式
输入占一行，将 3×3 的初始网格描绘出来。
例如，如果初始网格如下所示：
1 2 3 
x 4 6 
7 5 8 
则输入为：1 2 3 x 4 6 7 5 8

### 输出格式
输出占一行，包含一个整数，表示最少交换次数。
如果不存在解决方案，则输出 −1。

### 思路分析

1. 图论中使用dx和dy转移单元格状态
2. 最小步数 采用 bfs进行求解
3. 利用queue存储 当前的一维网格状态 ，同时把四周没有遍历的情况push到队列中，直到队列归0或者到达目标状态则完成bfs 得到答案解。
4. 队列可以用 queue<string>
   直接存转化后的字符串
   dist数组用 unordered_map<string, int>
   将字符串和数字联系在一起，字符串表示状态，数字表示距离
5. 字符串下标转换为二维 ： x = k / 3，y = k  % 3
6. 二维下表转换为字符串下标： x * 3 + y；
7.每一次遍历四周位置需要对转移进行还原 不然是不合理的

> 代码
```js
#include <iostream>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

int bfs(string start)
{
    string end = "12345678x";
    queue<string> q;
    unordered_map<string,int> d;
    q.push(start);
    d[start] = 0;
    int dx[4] = {-1,0,1,0},dy[4] = {0,1,0,-1};
    //dfs
    while(q.size())
    {
        string t = q.front();
        q.pop();
        int distance = d[t];
        if(t == end) return distance;
        //找出x在t字符串中的位置k
        int k = t.find('x');
        //把一维k下标转换为二维下标（x，y）
        int x = k / 3,y = k % 3;
        for(int i = 0;i < 4;i++)
        {
            int a = x + dx[i],b = y + dy[i];
            if(a >= 0 && a < 3 && b >= 0 && b < 3)
            {
                //转移x与周边的位置
                swap(t[k],t[a * 3 + b]);
                //当前状态如果是第一次遍历，记录距离并把字符串入队
                if(!d.count(t))
                {
                    d[t] = distance + 1;
                    q.push(t);
                }
                //还原状态，为下一种转换情况做准备
                swap(t[k], t[a * 3 + b]);
            }
        }
    }
    return -1;
}

int main()
{
    string c,start;
    for(int i = 0;i < 9;i++)
    {
        cin >> c;
        start += c;
    }
    cout << bfs(start) << endl;
    return 0;
}
```