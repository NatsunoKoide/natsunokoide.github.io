### ScrollView组件 

### 作用和制作
1）ScrollView是什么
滚动视图
我们现在用于编程的VS代码窗口就是典型的滚动视图
游戏中主要用于 背包、商店、排行榜等等功能
2）制作ScrollView
1.直接工具栏创建即可 NGUI——Create——ScrollView
2.若需要ScrollBar 自行添加水平和竖直

> 第三点非常重要 如果不添加脚本和碰撞器 会拖不动

3.添加子对象 为子对象添加Drag Scroll View和碰撞器

### 参数
![参数1](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/d6218278-b9fc-4b5d-9ce8-af018df870bd)
![参数2](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/b323daee-18d1-458b-b398-80fa84903770)
![参数3](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/1eba1c22-06de-4837-be6b-87e0fb3b223d)

### 设置30格背包格子（自己定义位置信息+更新滚动条的api）
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BagPanel : MonoBehaviour
{
    private static BagPanel instance;
    public static BagPanel Instance => instance;

    private void Awake()
    {
        instance = this;
    }

    public UIScrollView sv;
    public UIButton btnClose;

    private void Start()
    {
        //按下关闭按钮 隐藏界面
        btnClose.onClick.Add(new EventDelegate(() =>
        {
            this.gameObject.SetActive(false);
        }));

        //动态创建30格背包格子
        for (int i = 0; i < 60; i++)
        {
            GameObject obj = Instantiate(Resources.Load<GameObject>("Item"));
            obj.transform.SetParent(sv.transform, false);
            //通过自己的代码逻辑设置框的位置
            // % + / 非常经典的一维转二维位置
            obj.transform.localPosition = new Vector3(120 * (i % 5), 120 * (i / 5), 0);
        }

        //通过sv控制 滚动条更新
        sv.UpdateScrollbars();

        this.gameObject.SetActive(false);
    }
}

```