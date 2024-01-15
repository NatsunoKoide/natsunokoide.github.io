### Resources资源动态加载

### 1. Resources资源动态加载的作用
1. 通过代码动态加载Resources文件夹下指定路径资源
2. 避免繁琐的拖曳操作

### 2.常用资源类型
1.预设体对象——GameObject
2.音效文件——AudioClip
3.文本文件——TextAsset
4.图片文件——Texture
5.其它类型

> 预设体对象加载需要实例化,其它资源加载一般直接用

### 3.资源同步加载 普通方法

1. 在一个工程当中 Resources文件夹 可以有多个 通过API加载时 它会自己去这些同名的Resources文件夹中去找资源 打包时 Resources文件夹 里的内容 都会打包在一起.
2. 预设体对象:第一步：要去加载预设体的资源文件(本质上 就是加载 配置数据 在内存中) 第二步：如果想要在场景上 创建预设体 一定是加载配置文件过后 然后实例化
```js
Object obj = Resources.Load("Cube");
Instantiate(obj);
```
_预设体的初始化操作是必须的_

3.音效资源:第一步：就是加载数据 第二步：使用数据 我们不需要实例化 音效切片 我们只需要把数据 赋值到正确的脚本上即可
```js
public AudioSource audioS;
Object obj3 = Resources.Load("Music/BKMusic");
 audioS.clip = obj3 as AudioClip;
audioS.Play();
```

4.文本资源:支持主流格式包括 txt,xml,bytes,json,html,csv……
```js
 TextAsset ta = Resources.Load("Txt/Test") as TextAsset;
 print(ta.text);
```

5.图片资源
```js
private Texture tex;
tex = Resources.Load("Tex/TestJPG") as Texture;
```

6.资源同名问题
显然Resources.Load加载同名资源时 无法准确加载出你想要的内容
解决方案有：
   (1)加载指定类型的资源
```js
 tex = Resources.Load("Tex/TestJPG", typeof(Texture)) as Texture;
```
   (2)加载指定名字的所有资源
```js
 Object[] objs = Resources.LoadAll("Tex/TestJPG");
        foreach (Object item in objs)
            if (item is Texture)
            else if (item is TextAsset)
```

### 4. 解决资源类型和同名问题   泛型方法

> unity在Resources中提供了泛型接口 直接在使用的时候定义好类型即可
```js
TextAsset ta2 = Resources.Load<TextAsset>("Tex/TestJPG");
print(ta2.text);
tex = Resources.Load<Texture>("Tex/TestJPG");
```