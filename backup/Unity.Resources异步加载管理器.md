### Resources异步加载管理器

### 单例模式实现资源异步加载管理
**用于对象化resources资源的异步加载，使得其他脚本在加载资源的时候更加便利，只需要输入地址和结束后需要响应的函数即可**

> 异步资源加载管理器代码（ResourcesMgr）
_运用单例模式进行编写，由于没有继承mono所以instance需要实例化_
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public class ResourcesMgr 
{
    private static ResourcesMgr instance = new ResourcesMgr();
    public static ResourcesMgr Instance => instance;
    private ResourcesMgr(){}；

    //在Resources类中 对于LoadAsync函数中的泛型T有object约束 所以a as ResourceRequest 需要定义函数内变量的约束
    //public static ResourceRequest LoadAsync<T>(string path) where T : Object;
    //泛型unity委托是有参委托
    public void LoadRes<T>(string address,UnityAction<T> callBack) where T : Object
    {
        ResourceRequest rq = Resources.LoadAsync(address);
        rq.completed += (a) =>
        {
            callBack((a as ResourceRequest).asset as T);
        };
    }
}
```

> 演示如何在text脚本中使用
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson18Text : MonoBehaviour
{
    private Texture tex;
    // Start is called before the first frame update
    void Start()
    {
        ResourcesMgr.Instance.LoadRes<Texture>("软考", (obj) =>
        {
            tex = obj;
        });

    }

    private void OnGUI()
    {
        if(tex != null)
            GUI.DrawTexture(new Rect(0, 0, 100, 100), tex);
    }
```