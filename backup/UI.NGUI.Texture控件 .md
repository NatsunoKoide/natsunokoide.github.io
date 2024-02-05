### Texture控件 

### 基本参数
![texture控件](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/bcda5c29-4928-4824-85a1-af5cddbfb27a)

### 代码
```js
    public UITexture tex;
    void Start()
    {
        #region Texture用来干啥
        //Sprite只能显示图集中图片 一般用于显示中小图片
        //如果使用大尺寸图片 没有必要打图集
        //直接使用Texture组件进行大图片显示
        #endregion

        #region  代码设置
        //加载图片 
        Texture texture = Resources.Load<Texture>("BK");
        //改变图片
        if (texture != null)
            tex.mainTexture = texture;
        #endregion
    }
```

### 细节
创建texture 将图片放入需要查看图片的原始分辨率并进行调整
若为背景大图 需要对UIRoot组件进行 修改 保证 大图的清晰与准确