### 缓存池（对象池）
### 作用
**缓存池（对象池）的主要作用是优化资源管理，提高程序性能。
主要通过重复利用已经创建的对象，避免频繁的创建和销毁过程，
从而减少系统的内存分配和垃圾回收带来的开销。**

### 缓存池的基本实现

> 缓存池管理器
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// 缓存池模块 管理器
/// </summary>
public class PoolMgr : BaseManager<PoolMgr> //BaseManager<PoolMgr>继承 不继承mono的单例管理器
{
    //柜子容器放抽屉
    private Dictionary<string, Stack<GameObject>> poolDic = new Dictionary<string, Stack<GameObject>>();
    public GameObject GetObj(string name)
    {
        GameObject obj;
        //有抽屉 并且 抽屉里有对象
        if(poolDic.ContainsKey(name) && poolDic[name].Count > 0)
        {
            obj = poolDic[name].Pop();
            //激活对象 再返回
            obj.SetActive(true);
        }
        //否则去创建
        else 
        {
            obj = GameObject.Instantiate(Resources.Load<GameObject>(name));
        }
        return obj;
    }

    /// <summary>
    /// 往缓存池放入对象
    /// </summary>
    /// <param name="name">抽屉名字</param>
    /// <param name="obj">放入的对象</param>
    public void PushObj(string name,GameObject obj)
    {
        //不直接移除对象 而是将对象失活 用的时候再激活
        obj.SetActive(false);
        //没有抽屉 创建抽屉
        if (!poolDic.ContainsKey(name)) poolDic.Add(name, new Stack<GameObject>());
        poolDic[name].Push(obj);

        #region pushobj基础逻辑
        /*//如果存储对应的抽屉 直接放入
        if (poolDic.ContainsKey(name))
        {
            poolDic[name].Push(obj);
        }
        //没找到抽屉 创建一个出来 再放
        else
        {
            poolDic.Add(name, new Stack<GameObject>());
            poolDic[name].Push(obj);
        }*/
        #endregion

    }

    public void ClearPool()
    {
        poolDic.Clear();
    }
}
```

> 对象销毁脚本（挂载在预设体上）（使用缓存池”销毁“）
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DelayRemove : MonoBehaviour
{
    public string poolName;

    // Start is called before the first frame update
    //当对象激活的时候执行的生命周期函数
    private void OnEnable()
    {
        //延时函数
        Invoke("RemoveMe", 1f);
    }

    private void RemoveMe()
    {
        PoolMgr.Instance.PushObj(poolName, this.gameObject);
    }
}
```
