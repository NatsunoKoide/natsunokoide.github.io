### 题目
给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。
         7
      3   8
    8   1   0
  2   7   4   4
4   5   2   6   5

### 输入格式
第一行包含整数 n，表示数字三角形的层数。
接下来 n行，每行包含若干整数，其中第 i行表示数字三角形第 i层包含的整数。

### 输出格式
输出一个整数，表示最大的路径数字和。

### 数据范围
1≤n≤500,
−10000≤三角形中的整数≤10000

### 方法1 自上而下思想
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/759928da-34b8-4d03-a732-a0bca9f65096)

> 代码
```js
#include <iostream>
#include <cstring>
using namespace std;

const int N = 510;
int f[N][N];
int a[N][N];

int main()
{
    int n;
    cin >> n;
    for(int i = 1;i <= n;i++)
    {
        for(int j = 1;j <= i;j++)
        {
            cin >> a[i][j];
        }
    }
    //由于存在负权边，需要对答案数组进行一个初始化保证max函数的有效性
    memset(f,-0x3f,sizeof(f));
    f[1][1] = a[1][1];
    for(int i = 2;i <= n;i++)
    {
        for(int j = 1;j <= i;j++)
        {
            f[i][j] = a[i][j] + max(f[i-1][j-1],f[i-1][j]);
        }
    }
    //把答案也要初始化为负无穷
    int res = -0x3f3f3f3f;
    for(int  i = 1;i <= n;i++)res = max(res,f[n][i]);
    cout << res <<endl;
    return 0;
}
```

### 方法2 自下而上的思想
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/5b8cab35-9efe-490e-be55-7b0680835ed5)

```js
#include <iostream>
using namespace std;
const int N = 510;
int n;
int a[N][N];
int f[N][N];
int main () {
    cin >> n;
    for (int i = 1;i <= n;i++) {
        for (int j = 1;j <= i;j++) cin >> a[i][j];
    }
    for (int i = 1;i <= n;i++) f[n][i] = a[n][i];
    for (int i = n - 1;i >= 1;i--) {
        for (int j = 1;j <= i;j++) {
            f[i][j] = max (f[i + 1][j],f[i + 1][j + 1]) + a[i][j];
        }
    }
    cout << f[1][1] << endl;
    return 0;
}
```