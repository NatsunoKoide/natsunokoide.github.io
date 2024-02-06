### Toggle单选多选组件

### 参数
![toggle参数](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/5b2f6a06-f73d-4446-b978-3a9db45445a5)

### 代码与解析
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    public UIToggle tog1;
    public UIToggle tog2;
    public UIToggle tog3;

    void Start()
    {
        #region 知识点 Toggle用来干啥
        //单选框 多选框都可以使用它来制作
        #endregion

        #region 知识点 制作Toggle
        //1.2个Sprite 1父1子
        //2.为父对象添加Toggle脚本
        //3.添加碰撞器
        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖代码（button类似）
        //2.代码进行监听添加
        tog1.onChange.Add(new EventDelegate(Change2));
        tog2.onChange.Add(new EventDelegate(Change2));
        tog3.onChange.Add(new EventDelegate(Change2));
        #endregion
    }

    private void Change2()
    {
        print("代码监听");
    }

    //可以将toggle设置为同一级 这样就可以实现if~else if的单选判断
    public void Change()
    {
        print("Toggle变化执行的内容");

        if (tog1.value)
        {
            print("tog1选中");
        }
        else if (tog2.value)
        {
            print("tog2选中");
        }
        else if (tog3.value)
        {
            print("tog3选中");
        }
    }
}

```