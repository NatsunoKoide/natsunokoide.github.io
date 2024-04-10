### UI.Demo.面板管理类

### 内容
1. 单例模式
2. 构造函数管理画布获取以及过场景不移除画布的功能
3. 显示i面板
4. 隐藏面板
5. 获取面板

_该管理类非常重要，多思多想，难度不大_

> 代码
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//管理器 一般都是单例模式
public class UIManager : MonoBehaviour
{
    private static UIManager instace; 
    public static UIManager Instance => instace;

    //存储面板的容器
    private Dictionary<string, BasePanel> panelDic = new Dictionary<string, BasePanel>();
    //得到Canvas对象
    private Transform canvasTrans;

    //在构造函数里获取画布对象的位置信息 以及 不让画布对象被移除
    private UIManager()
    {
        //视频中的写法
        /*//得到场景上创建好的 Canvas对象
        canvasTrans = GameObject.Find("Canvas").transform;
        //让 Canvas对象 过场景 不移除 
        //我们都是通过 动态创建 和 动态删除 来显示 隐藏面板的 所以不删除它 影响不大
        GameObject.DontDestroyOnLoad(canvasTrans.gameObject);*/

        //自己的写法
        GameObject canvasObj = GameObject.Find("Canvas");
        if (canvasObj != null) canvasTrans = canvasObj.transform;
        DontDestroyOnLoad(canvasObj);
    }

    //显示面板
    public T ShowPanel<T>() where T : BasePanel
    {
        //规定 泛型T 的类型和面板名一致 （方便使用）
        string panelName = typeof(T).Name;
        //判断是否已经存在该面板 如果有 不用创建 返回外部直接使用
        if (panelDic.ContainsKey(panelName)) return panelDic[panelName] as T;

        //显示面板的核心逻辑就是 动态创建界面预制体 并 设置 位置父对象
        GameObject panelObj = GameObject.Instantiate(Resources.Load<GameObject>("UI/" + panelName));
        panelObj.transform.SetParent(canvasTrans,false);

        //获取面板预制体上的脚本
        T panel = panelObj.GetComponent<T>();
        //把脚本存储到对应的字典中
        panelDic.Add(panelName, panel);

        //调用显示自己的逻辑
        panel.ShowMe();
        return panel;
    }

    //隐藏面板
    //参数：isFade表示是否需要淡入淡出
    public void HidePanel<T>(bool isFade = true) where T : BasePanel
    {
        //通过规则+泛型得到面板的名字
        string panelName = typeof(T).Name;
        //判断当前 有没有该名字的面板
        if(panelDic.ContainsKey(panelName))
        {
            if(isFade)
            {
                panelDic[panelName].HideMe(() =>
                {
                    //面板淡出后执行删除面板
                    GameObject.Destroy(panelDic[panelName].gameObject);
                    //删除面板的同时 在字典里把 该面板清楚
                    panelDic.Remove(panelName);
                });
            }
            else
            {
                GameObject.Destroy(panelDic[panelName].gameObject);
                panelDic.Remove(panelName);
            }
        }
    }

    //获取面板
    public T GetPanel<T>() where T : BasePanel
    {
        //通过规则+泛型得到面板的名字
        string panelName = typeof(T).Name;
        //判断是否已经存在该面板 如果有 不用创建 返回外部直接使用
        if (panelDic.ContainsKey(panelName)) return panelDic[panelName] as T;
        //如果没有该泛型（名字）的面板 则返回空
        return null;
    }

}

```