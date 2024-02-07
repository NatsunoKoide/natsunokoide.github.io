### PopupList组件 

### 参数
![参数1](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/5f1e4f50-e74b-40c0-ad2b-0a6a976590f2)
![参数2](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/c377a1f7-da85-458c-b0db-39d57a150217)

### 代码与解析
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public UIPopupList list;
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点 PopupList是啥？
        //下拉列表 
        #endregion

        #region 知识点 制作Popuplist
        //1.一个sprite做背景 一个lable做显示内容
        //2.添加PopupList脚本
        //3.添加碰撞器
        //4.关联lable做信息更新，选择Label中的SetCurrentSelection函数
        #endregion

        #region 知识点 添加新选项 + 监听事件的两种方式
        //1.拖曳代码
        //2.代码关联
        list.items.Add("新加 选项4");

        list.onChange.Add(new EventDelegate(() => {

            print("代码添加的监听" + list.value);
        }));
        #endregion
    }

    public void OnChange()
    {
        print("选项变化" + list.value);
    }
}

```