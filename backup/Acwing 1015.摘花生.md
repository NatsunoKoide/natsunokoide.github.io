### 题目
Hello Kitty想摘点花生送给她喜欢的米老鼠。
她来到一片有网格状道路的矩形花生地(如下图)，从西北角进去，东南角出来。
地里每个道路的交叉点上都有种着一株花生苗，上面有若干颗花生，经过一株花生苗就能摘走该它上面所有的花生。
Hello Kitty只能向东或向南走，不能向西或向北走。
问Hello Kitty最多能够摘到多少颗花生。
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/125cb338-ca0c-4f99-9538-cecf6087b0b2)

### 输入格式
第一行是一个整数T，代表一共有多少组数据。
接下来是T组数据。
每组数据的第一行是两个整数，分别代表花生苗的行数R和列数 C。
每组数据的接下来R行数据，从北向南依次描述每行花生苗的情况。每行数据有C个整数，按从西向东的顺序描述了该行每株花生苗上的花生数目M。

### 输出格式
对每组输入数据，输出一行，内容为Hello Kitty能摘到得最多的花生颗数。

### 数据范围  —— N = 100

### 分析
1.数字三角形dp 
2.状态分析——两种 f[i-1][j]+w[i][j] 和 f[i][j-1]+w[i][j] 分别代表从左侧和从上侧


> 代码
```js
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;
int n,m;
int w[N][N];
int f[N][N];

int main()
{
   int T;
   cin >> T;
   while(T--)
   {
       cin >> n >> m;
       for(int i = 1;i <=n;i++)
       {
           for(int j = 1;j <= m;j++)
           {
               cin >> w[i][j];
           }
       }
       for(int i = 1;i <= n;i++)
       {
           for(int j= 1;j <= m;j++)
           {
               f[i][j] = max(f[i-1][j],f[i][j-1]) + w[i][j];
           }
       }
       cout << f[n][m] << endl;
   }
   return 0;
}
```
