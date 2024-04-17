### 公共Mono模块
### 实现功能
 （1）实现一个单例模式的Mono模块
 （2）能使其他没有继承Mono的脚本 进行帧更行等操作
 （3）本质上是使不继承mono的脚本在公共Mono模块中添加事件
 （4）由于公共Mono模块是继承了单例模式管理类的所以自带Mono的功能所以还可以实现协程的功能（Coroutine）
          实现方法是在不继承Mono的脚本中 用公共Mono模块点出协程使用：MonoMgr.Instance.StartCoroutine(Test());

> 代码
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

/// <summary>
/// 公共Mono模块管理器
/// </summary>
public class MonoMgr : SingleAutoMono<MonoMgr>
{
    private event UnityAction updateEvent;
    private event UnityAction fixedUpdateEvent;
    private event UnityAction lateUpdateEvent;

    //添加更新监听函数
    public void AddUpdateListener(UnityAction updateFun)
    {
        updateEvent += updateFun;
    }
    public void AddFixedUpdateListener(UnityAction updateFun)
    {
        fixedUpdateEvent += updateFun;
    }
    public void AddLateUpdateListener(UnityAction updateFun)
    {
        lateUpdateEvent += updateFun;
    }

    //移除更新函数
    public void RemoveUpdateListener(UnityAction updateFun)
    {
        updateEvent -= updateFun;
    }
    public void RemoveFixedUpdateListener(UnityAction updateFun)
    {
        fixedUpdateEvent -= updateFun;
    }
    public void RemoveLateUpdateListener(UnityAction updateFun)
    {
        lateUpdateEvent -= updateFun;
    }

    private void Update()
    {
        updateEvent?.Invoke();
    }

    private void FixedUpdate()
    {
        fixedUpdateEvent?.Invoke();
    }

    private void LateUpdate()
    {
        lateUpdateEvent?.Invoke();
    }
}

```

> 测试脚本
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//没有继承mono，测试利用monomgr实现无mono的子脚本更新帧
public class Test6Mgr : BaseManager<Test6Mgr>
{
    //定义一个协程用于协程赋值
    private Coroutine testFun;

    public void IcanUpdate()
    {
        MonoMgr.Instance.AddUpdateListener(MyUpdate);
        testFun = MonoMgr.Instance.StartCoroutine(Test());
    }

    public void IcanStopUpdate()
    {
        MonoMgr.Instance.RemoveUpdateListener(MyUpdate);
        MonoMgr.Instance.StopCoroutine(testFun);
    }

    private IEnumerator Test()
    {
        yield return new WaitForSeconds(3f);
        Debug.Log("TestTeat");
    }

    private void MyUpdate()
    {
        Debug.Log("1231312");
    }
}
```

> main函数
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Main : MonoBehaviour
{
    private void Start()
    {
    }


    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
            Test6Mgr.Instance.IcanUpdate();

        if (Input.GetKeyUp(KeyCode.Space))
            Test6Mgr.Instance.IcanStopUpdate();
    }
}
```