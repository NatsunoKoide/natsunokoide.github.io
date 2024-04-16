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
    //提供给外部一个bool变量用于判断是否已经存在单例模式
    //程序员在子类里利用这个bool进行判断即可
    protected bool InstanceIsNull => instane == null;
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
                //还有一种方法也可以实现私有构造函数实例化对象
                //instance = Activator.CreateInstance(typeof(T), true) as T;
                if (info == null) Debug.LogError("没有对应的无参构造函数");
            }
            return instance;
        }
    }
}

```

### 安全性优化后的无Mono单例模式基类 （加锁的版本）
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
    //readonly：这表明一旦 lockObj 在构造函数中或者在声明时被初始化之后，就不能被修改。这是一个好的做法，因为锁对象不应该被更换，一旦创建应保持不变。
    //用于加锁的对象
    protected static readonly object lockObj = new object();
    public static T Instance
    {
        get
        {
            //双重判空提升性能
            if(instance == null)
            {
                lock (lockObj)
                {
                    if (instance == null)
                    {
                        //利用 反射 得到无参私有构造函数 用来对象的实例化
                        Type type = typeof(T);
                        ConstructorInfo info = type.GetConstructor(BindingFlags.Instance | BindingFlags.NonPublic, null, Type.EmptyTypes, null);
                        instance = info?.Invoke(null) as T;
                        //还有一种方法也可以实现私有构造函数实例化对象
                        //instance = Activator.CreateInstance(typeof(T), true) as T;
                        if (info == null) Debug.LogError("没有对应的无参构造函数");
                    }
                }
            }
            return instance;
        }
    }
}
```

### 补充： 以上均为 懒汉单例模式
### 补充 ：饿汉单例模式 就算 属性直接=》instance 
### 补充 ：区别：

懒字体现在：这种单例模式只会在第一次使用时才创建事例，而不是应用程序启动时就创建。一种“敌不动我不动”，“催一下动一下”的感觉
它的好处也在于此，因为只有当我们代码中需要用到某个单例模式对象时才会去实例化分配内存。
在商业项目中，游戏系统是非常多的，内存开销也是较大的，懒汉模式的延迟实例化特点可以帮助我们在一定程度上缓解一丁点的内存压力。

饿字的体现在：这种单例模式在应用程序启动时就创建事例，无论是否使用该事例。相当于不管我“吃不吃”，我就要创建它，先放在那，一种“饥不择食”的感觉。
虽然看起来“饿汉”没有“懒汉”那么好，但实际上，“饿汉”最大的优点就是“懒汉”的缺点，因为“饿汉”不用在实例化时考虑线程安全问题，它具有天生的线程安全。而“懒汉”在我们之前讲解的课程当中，大家已经感受到了，我们需要考虑多线程的并发访问问题。