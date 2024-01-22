### 异步加载场景

> 代码以及总结
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Lesson20 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 回顾场景同步切换
        //SceneManager.LoadScene("Lesson20test");


        //需要在build settings中添加场景才能使用

        //同步切换的缺点：
        //1.unity会删除当前场景的所有对象
        //2.同时加载下一个场景的相关信息
        //3.当前对象过多或者下一个场景对象过多时 会非常耗时并造成卡顿

        //对于切换的卡顿 可以用异步切换解决这个问题
        #endregion

        #region 知识点二 场景异步加载
        //1.通过事件回调函数 实现异步加载
        //AsyncOperation oa =  SceneManager.LoadSceneAsync("Lesson20test");
        //当场景异步加载结束后 自动调用事件函数 
        //如果希望在加载之后做一些说明 可以把逻辑加入到事件中
        //oa.completed += (a) =>
        //{
        //    print("加载结束");
        //};
        //oa.completed += LoadOver;


        //2.通过协程异步加载
        //加载场景会把当前场景上 没有特殊处理的对象都删除
        //所以协程中部分逻辑 可能执行部不了

        //解决办法：让处理场景加载的脚本衣服的对象 过场景时 不被移除
        //         使用函数 DontDestroyOnLoad（this.gameobject）;

        DontDestroyOnLoad(this.gameObject);
        StartCoroutine(LoadScenes("Lesson20test"));
        #endregion
    }

    private void LoadOver(AsyncOperation ao)
    {
        print("loadover");
    }

    IEnumerator LoadScenes(string name)
    {
        AsyncOperation ao = SceneManager.LoadSceneAsync(name);
        //unity内部会发现是异步加载类型的返回对象 
        //会等待异步加载结束后 才会执行迭代器之后的内容
        //协程的好处 是异步加载场景时 可以在加载的同时做一些别的逻辑
        print("异步加载中");
        //yield return ao;
        //print("异步加载后");


        //可以在异步加载过程中 更新进度条
        //第一种 利用场景异步加载的进度 更新 但不准确 不常用
        while(!ao.isDone)
        {
            print(ao.progress);
            yield return null;
        }
        //离开循环 意味着加载结束 进度套满 完成切换 然后对进度条隐藏


        //第二种 根据游戏规则 自己定义 进度条变化的条件
        yield return ao;
        //场景加载结束 更新20%
        //接着加载场景的其他信息 比如加载怪物
        //再更新20% 
        //接着加载场景模型 ……………………
    }
}

#region 总结
//场景异步加载 和 资源异步加载 一样
//有两种方式
//1.通过事件回调函数
//2.协程异步加载

//他们的优缺点表现和资源异步加载 也是一样的
//1.事件回调函数
//优点：写法简单，逻辑清晰
//缺点：只能加载完场景做一些事情 不能再加载过程中处理逻辑
//2.协程异步加载
//优点：可以在加载过程中处理逻辑，比如进度条更新等
//缺点：写法较为麻烦，要通过协程
#endregion

```

### 写一个异步加载场景的管理器

> 管理器代码
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.SceneManagement;

public class SceneMgr 
{
    private static SceneMgr instance = new SceneMgr();
    public static SceneMgr Instance => instance;
    private SceneMgr() { }

    public void LoadScene(string name,UnityAction action)
    {
        AsyncOperation ao = SceneManager.LoadSceneAsync(name);
        //只是completed加入的委托一定要给予一个参数 此处a并没有用到
        ao.completed += (a) =>
         {
             //通过lamda表达式包裹一层 在内部直接调用外部传入的委托
             action();
         };
    }
}
```

> 管理器测试脚本
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson20Test : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        SceneMgr.Instance.LoadScene("Lesson20test", () => 
        {
            print("加载结束");
        });
    }
}
```