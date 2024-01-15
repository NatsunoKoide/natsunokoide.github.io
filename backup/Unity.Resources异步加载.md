### Resources异步加载
### 对于异步加载的定义是什么
如果加载过大的资源可能会造成程序卡顿
卡顿的原因就是 从硬盘上把数据读取到内存中 是需要进行计算的
越大的资源耗时越长，就会造成掉帧卡顿

> Resources异步加载 就是内部新开一个线程进行资源加载 不会造成主线程卡顿

### Resources异步加载方式

1. 通过ResourceRequest中的事件实现异步加载
```js
// Unity 在内部 实例化一个rq请求 这个请求会去开一个线程进行资源下载
ResourceRequest rq = Resources.LoadAsync<Texture>("Tex/TestJPG");
//对资源下载结束这一事件进行监听
rq.completed += LoadOver;
//不能直接在 ResourceRequest rq 下一行直接调用 rq.asset 此时资源还没有加载完
```
```js
//completed事件需要形参 AsyncOperation 也就是正在监听是否结束的事件
private void LoadOver(AsyncOperation rq)
    {
        print("加载结束");
        //asset 是资源对象 加载完毕过后 就能够得到它
        //需要将AsyncOperation转换为ResourceRequest才能点出asset资源属性才能 再从资源as为目标对象类型
        tex = (rq as ResourceRequest).asset as Texture;
    }
private void OnGUI()
    {
        //判定tex对象不为空后 进行gui绘制
        if (tex != null)
            GUI.DrawTexture(new Rect(0, 0, 100, 100), tex);
    }
```
2. 通过协程实现 异步加载资源
```js
void start()
{
     //在start函数中 开启协程 运行 协程函数load
     StartCoroutine(Load());
}
```
```js
 IEnumerator Load()
    {
        //迭代器函数 当遇到yield return时  就会停止执行之后的代码
        //协程协调器 通过得到 返回的值 去判断 下一次执行后面的步骤 将会是何时
        ResourceRequest rq = Resources.LoadAsync<Texture>("Tex/TestJPG");

        //第一部分
        //Unity 自己知道 该返回值 意味着你在异步加载资源 

        yield return rq; 
        //通过yield return 一个ResourceRequest 可以让协程知道在运行异步加载

        //Unity 会自己判断 该资源是否加载完毕了 加载完毕过后 才会继续执行后面的代码

         //另一种判断资源是否加载的方式 
        //判断资源是否加载结束
        while (!rq.isDone)
        {
            //打印当前的 加载进度 
            //该进度 不会特别准确 过渡也不是特别明显
            print(rq.progress);
            yield return null;
        }
        tex = rq.asset as Texture;

        //yield return null;
        ////第二部分
        //yield return new WaitForSeconds(2f);
        ////第三部分
    }
```