### 题目
动物王国中有三类动物 A,B,C，这三类动物的食物链构成了有趣的环形。
A吃 B，B 吃 C，C 吃 A。
现有 N个动物，以 1∼N 编号。
每个动物都是 A,B,C中的一种，但是我们并不知道它到底是哪一种。
有人用两种说法对这 N个动物所构成的食物链关系进行描述：
第一种说法是 1 X Y，表示 X 和 Y 是同类。
第二种说法是 2 X Y，表示 X吃 Y。
此人对 N个动物，用上述两种说法，一句接一句地说出 K句话，这 K句话有的是真的，有的是假的。
当一句话满足下列三条之一时，这句话就是假话，否则就是真话。
当前的话与前面的某些真的话冲突，就是假话；
当前的话中 X 或 Y比 N大，就是假话；当前的话表示 X吃 X，就是假话。
你的任务是根据给定的 N和 K句话，输出假话的总数。

### 输入格式
第一行是两个整数 N 和 K，以一个空格分隔。
以下 K行每行是三个正整数 D，X，Y，两数之间用一个空格隔开，其中 D 表示说法的种类。
若 D=1，则表示 X 和 Y 是同类。
若 D=2，则表示 X 吃 Y。

### 输出格式
只有一个整数，表示假话的数目。

### 数据范围
1≤N≤50000
0≤K≤100000

### 针对find函数提供递归模拟
![41774_bd485030c0-JIE](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/e48f6270-4db3-40bb-b878-9855ed8c0ba2)

> 代码 + 解析
```js
#include <iostream>

using namespace std;

const int N = 1e5+10;

int n,m;
//p[]寻找祖宗节点，d[]求到父节点的距离
int p[N],d[N];

//find函数用于数据压缩并将所有子节点都指向根节点
int find(int x)
{
    if(p[x] != x)
    {
        int t = find(p[x]);
        //find里面dx更新的是自己到祖父节点的距离（受递归影响，会一路往上加）
        d[x] += d[p[x]];
        //把路径压缩结果赋值给px
        p[x] = t;
    }
    return p[x];
}

int main()
{
    cin >> n >> m;
    for(int i = 1;i <= n;i++) p[i] = i; //将每一个动物都构建成一个以自己为根的节点
    int res = 0; //记录错误
    while(m--)
    {
        int t,x,y;
        scanf("%d%d%d",&t,&x,&y);
        if(x > n || y > n) res++;//出现超出编号的动物是谎言
        else
        {
            //记录 x,y的根节点
            int px = find(x),py = find(y);
            if(t == 1) //说法1
            {
                if(px == py)
                {
                    //(d[x] - d[y]) % 3如果不是0证明x，y不是同一类，那么这句话是谎言
                    if((d[x] - d[y]) % 3) res ++;
                }
                else
                {
                    //x,y不在一个集合中
                    p[px] = py; //把y的祖父节点接到x的祖父节点上
                    //d[px] = d[y] - d[x];的原理是模拟当满足px和py为同类时 合并到一起 dpx距离为dy-dx时 % 3 为0
                    d[px] = d[y] - d[x];
                    // d[x] 的距离为什么不更新？
                    // 在用 find 中更新
                }
            }
            else //说法2
            {
                if(px == py)
                {
                    // 若 X 吃 Y，则 d[x] 比 d[y] 大 1
                    if((d[x] - d[y] - 1) % 3) res ++;
                }
                else
                {
                    p[px] = py;
                    // 满足 d[x] + d[px] - 1 = d[y] 的时候 可以满足px和py不是同类且x吃y
                    //此处的+1 是dx数组的关键，因为当最开始没有一个动物合并集合的时候都会来这里+1
                    d[px] = d[y] + 1 - d[x];
                }
            }
        }
    }
    printf("%d\n", res);

    return 0;
}
```
