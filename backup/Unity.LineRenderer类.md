### LineRenderer类

> 相关介绍以及常用API说明
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson21 : MonoBehaviour
{
    private Material m;
    void Start()
    {
        #region LineRenderer是什么
        //LineRenderer是Unity提供的一个用于画线的组件
        //使用它我们可以在场景中绘制线段
        //一般可以用于
        //1绘制攻击范围
        //2武器红外线
        //3辅助功能
        //4其它画线功能
        #endregion

        #region LineRender代码相关  如果忘记了api在lineRenderer类里面自己找
        //动态添加一个线段，空物体
        GameObject line = new GameObject();
        //把空物体对象改名为Line
        line.name = "Line";
        //代码挂载LineRenderer
        LineRenderer lineRenderer = line.AddComponent<LineRenderer>();

        //首尾相连
        lineRenderer.loop = true;

        //开始结束宽
        lineRenderer.startWidth = 0.02f;
        lineRenderer.endWidth = 0.02f;

        //开始结束颜色
        lineRenderer.startColor = Color.white;
        lineRenderer.endColor = Color.red;

        //设置材质
        //使用Resources同步加载
        m = Resources.Load<Material>("M");
        lineRenderer.material = m;

        //设置点
        //一定注意 设置点 要 先设置点的个数
        lineRenderer.positionCount = 4;
        //接着就设置 对应每个点的位置
        //如果设置的点少于positionCount的个数 unity会用0，0，0点填充
        lineRenderer.SetPositions(new Vector3[] { new Vector3(0,0,0),
                                                  new Vector3(0,0,5),
                                                  new Vector3(5,0,5)});
        lineRenderer.SetPosition(3, new Vector3(5, 0, 0));

        //是否使用世界坐标系
        //决定了 是否随对象移动而移动
        lineRenderer.useWorldSpace = false;

        //让线段受光影响 会接受光数据 进行着色器计算
        lineRenderer.generateLightingData = true;
        #endregion
    }
}
```

### LineRenderer类在Unity编辑器中的参数 （出自唐老狮课件）
![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/4b4460ce-ebc3-48af-94b8-cb5e4bdea61d)

![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/422257e6-06c6-49e6-80b4-d526b60348e0)

![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/47c91c30-901d-4d5b-a2b4-6e411c0dcb64)

![image](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/00a605af-0d59-40b2-88b8-b43ad0b77983)


