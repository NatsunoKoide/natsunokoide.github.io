### UI.Demo.面板基类

### 内容

1. 唤醒界面 触发 画布组件判断
2. 写一个初始化抽象方法给之后的界面继承
3. 写显示和关闭（淡入淡出的函数）
4. 淡出时 添加事件用于 让管理器删除界面
5. 淡入淡出的update逻辑（包括速度和alpha判断）

> 代码
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public abstract class BasePanel : MonoBehaviour
{
    //画布组件——用于控制界面淡入淡出
    private CanvasGroup canvasGroup;
    //淡入淡出效果速度
    private float alphaSpeed = 10;
    //是否显示
    private bool isShow;
    //当自己淡出成功时 要执行委托函数
    private UnityAction hideCallBack;

    protected virtual void Awake()
    {
        //获取画布组件，如果画布组件为null，代码加入组件
        canvasGroup = GetComponent<CanvasGroup>();
        if (canvasGroup == null) canvasGroup = gameObject.AddComponent<CanvasGroup>();
    }

    protected virtual void Start()
    {
        Init();
    }

    public abstract void Init();

    public virtual void ShowMe()
    {
        isShow = true;
        canvasGroup.alpha = 0;
    }

    public virtual void HideMe(UnityAction callBack)
    {
        isShow = false;
        canvasGroup.alpha = 1;
        //记录传入的 当淡出成功后 会执行的函数
        hideCallBack = callBack;
    }

    // Update is called once per frame
    void Update()
    {
        //淡入效果
        if(isShow && canvasGroup.alpha != 1)
        {
            canvasGroup.alpha += alphaSpeed * Time.deltaTime;
            if (canvasGroup.alpha >= 1) canvasGroup.alpha = 1;
        }
        //淡出效果
        else if(!isShow)
        {
            canvasGroup.alpha -= alphaSpeed * Time.deltaTime;
            if (canvasGroup.alpha <= 0)
            {
                canvasGroup.alpha = 0;
                //管理器 删除 自己
                hideCallBack?.Invoke();
            }
        }
    }
}

```