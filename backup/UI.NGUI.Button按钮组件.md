### Button按钮组件

### 参数
![按钮参数](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/9882d3df-1237-497e-a8d2-ae6b9af0ae9c)

### 代码
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    public UIButton btn;

    void Start()
    {
        #region 知识点 所有组合控件的共同特点
        //1.在3个基础组件对象上添加对应组件
        //2.如果希望响应点击等事件 需要添加碰撞器
        #endregion

        #region 知识点 Button是用来干嘛的
        //UI界面中的按钮 当点击按钮后我们可以进行一些处理
        #endregion

        #region 知识点 制作Button
        //1.一个Sprite（需要文字再加一个Label子对象）
        //2.为Sprite添加Button脚本
        //3.添加碰撞器
        #endregion

        #region 知识点 监听事件的两种方式
        //1.拖脚本
        //直接在unity编辑器中进行 把脚本托给Ngui对象然后在编辑器里面选
        //2.代码获取按钮对象监听
        btn.onClick.Add(new EventDelegate(ClickDo2));

        btn.onClick.Add(new EventDelegate(() => {
            print("那么大表达式添加的 点击事件处理");
        }));
        #endregion
    }

    public void ClickDoSomthing()
    {
        print("按钮点击");
    }

    public void ClickDo2()
    {
        print("按钮点击2");
    }

    #region 总结
    //1.button的制作流程
    //  3个基础组件构成 任意一个基础组件 往上面添加Button脚本 再添加碰撞器 就可以让它变成一个按钮
    //2.事件的监听
    // 通过 拖曳 或者 代码的形式 可以进行按钮的 点击事件 监听
    #endregion
}

```