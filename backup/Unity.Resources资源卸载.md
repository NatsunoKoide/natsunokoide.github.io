### Resources资源卸载

1. Resources是否存在重复加载资源以及内存消耗
Resources加载一次资源后，资源会存放在内存中作为缓存。
当第二次加载相同资源时，unity会直接取缓存中的资源进行使用，所以不会因为重复加载而浪费内存。但在性能上会有影响 （每次加载都会去查找取出，始终伴随一些性能消耗）。

2.卸载指定资源：
Resources.UnloadAsset 方法
该方法 不能释放 GameObject对象 因为它会用于实例化对象
它只能用于一些 不需要实例化的内容 比如 图片 和 音效 文本等等
**一般情况下 我们很少单独使用它**
```js
GameObject obj = Resources.Load<GameObject>("Cube");
即使是没有实例化的 GameObject对象也不能进行卸载
Resources.UnloadAsset(obj); //这个代码是无效的
```

3.  卸载未使用的资源
注意：
一般在过场景时和GC一起使用
```js
Resources.UnloadUnusedAssets();
GC.Collect();
```