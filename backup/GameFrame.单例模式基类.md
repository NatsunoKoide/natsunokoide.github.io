### 单例模式基类

### 不继承Mono的单例模式基类
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BaseManager<T> where T : class,new ()
{
    private static T instance;
    public static T Instance
    {
        get
        {
            if(instance == null)
            {
                instance = new T();
            }
            return instance;
        }
    }
}
```

### 继承Mono的单例模式基类（挂载式）
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// 挂载式 继承Mono 的单例模式基类
/// </summary>
public class SingletonMono<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T instance;
    public static T Instance => instance;
    protected virtual void Awake()
    {
        instance = this as T;
    }
}
```

### 继承Mono的单例模式基类（自动挂载式）
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// 自动挂载 继承Mono的单例模式基类 （推荐使用）
/// </summary>
/// <typeparam name="T"></typeparam>
public class SingleAutoMono<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T instance;
    public static T Instance
    {
        get
        {
            if(instance == null)
            {
                //动态创建 动态挂载
                //在场景上 创建 空物体
                GameObject obj = new GameObject();
                //得到T脚本的类名 为对象改名 在编辑器中可以更加明确的看到该单例模式脚本对象
                obj.name = typeof(T).ToString();
                //动态挂载对应的单例模式脚本
                instance = obj.AddComponent<T>();
                //过场景的时候不移除
                GameObject.DontDestroyOnLoad(instance);
            }
            return instance;
        }
    }

    protected virtual void Awake()
    {
        instance = this as T;
    }
}
```

### 安全性优化后的无Mono单例模式基类
```js
using System;
using System.Collections;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;

/// <summary>
/// 安全性优化后的无Mono单例模式基类
/// 1.class 前加 abstract 防止基类在其他地方被new
/// 2.利用反射的方式获取子类T中的构造函数信息，使用无参构造实例化instance
/// </summary>
/// <typeparam name="T"></typeparam>
public abstract class BaseManagerPro<T> where T : class
{
    private static T instance;
    public static T Instance
    {
        get
        {
            if(instance == null)
            {
                //利用 反射 得到无参私有构造函数 用来对象的实例化
                Type type = typeof(T);
                ConstructorInfo info = type.GetConstructor(BindingFlags.Instance | BindingFlags.NonPublic, null, Type.EmptyTypes, null);
                instance = info?.Invoke(null) as T;
                if (info == null) Debug.LogError("没有对应的无参构造函数");
            }
            return instance;
        }
    }
}

```