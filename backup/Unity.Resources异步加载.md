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
```
2. 通过协程实现 异步加载资源