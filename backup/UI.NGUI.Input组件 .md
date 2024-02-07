### Input组件 

### 参数
![input参数1](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/531345f0-7ac1-4ebf-9c3f-6cde944796de)
![input参数2](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/e52ef3c0-5b14-45a2-9a6f-c7f1eee0ea3a)

### 代码与解析
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{
    public UIInput input;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点 Input是啥
        //输入框
        //可以用来制作账号密码聊天输入框
        #endregion

        #region 知识点 制作Input
        //1.1个Sprite做背景 1个Label显示文字
        //2.为Sprint添加Input脚本
        //3.添加碰撞器
        #endregion

        #region 知识点 监听事件的两种方式
        //1.拖曳脚本
        //2.通过代码关联

        input.onSubmit.Add(new EventDelegate(() =>
        {
            print("完成输入 通关代码添加的监听函数");
        }));

        input.onChange.Add(new EventDelegate(() =>
        {
            print("输入变化 通关代码添加的监听函数");
        }));
        #endregion
    }

    public void OnSubmit()
    {
        print("输入完成" + input.value);
    }

    public void OnChange()
    {
        print("输入变化" + input.value);
    }
}
```