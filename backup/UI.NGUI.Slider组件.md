### Slider组件

### 参数
![参数](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/ccd56038-cc70-4eac-a9c5-93d91e27ab80)


> 注意Slider与之前学习组件不同多了一个委托函数slider.onDragFinished可以直接+=lamda表达式！！！！！！！！！！！！！！！！！！！！


### 代码与解析
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    public UISlider slider;
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点 Slider是啥
        //滑动条控件
        //主要用于设置音乐音效大小等
        #endregion

        #region 知识点 制作Slider
        //1.3个sprite 1个做根对象为背景  2个子对象 1个进度 1个滑动块 
        //2.设置层级
        //3.为根背景添加Slider脚本
        //4.添加碰撞器（父对象或者滑块）
        //5.关联3个对象
        #endregion

        #region 知识点 监听事件的两种方式
        //1.拖曳脚本关联
        //2.通过代码关联
        slider.onChange.Add(new EventDelegate(() => {

            print("通过代码监听" + slider.value);
        }));

        slider.onDragFinished += () => {
            print("拖曳结束" + slider.value);
        };
        #endregion
    }

    public void OnChange()
    {
        print("值变化" + slider.value);
    }
}

```