### Sprite控件

### 作用
NGUI中所有中小尺寸图片显示都用Sprite显示，使用它来显示图集中的单个图片资源

### 参数
![sprite参数1](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/b745b945-4b7f-4daf-8980-d8f7e0f94b07)
![sprite参数2](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/b0e6998e-9c62-4420-bee0-f2fc941de96e)
![sprite参数3](https://github.com/NatsunoKoide/natsunokoide.github.io/assets/137853852/146cb5b7-786b-4c0e-812e-93347c226da0)

### 代码相关内容
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson3 : MonoBehaviour
{
    //定义精灵图
    public UISprite sprite;
    void Start()
    {
        //设置长宽
        sprite.width = 200;
        sprite.height = 300;

        //改变为当前图集中选择的图片
        sprite.spriteName = "bk";

        //2.改变为其它图集中的图片
        //先加载图集
        //图集在 NGUIAtlas 类中 使用Resources直接加载
        NGUIAtlas atlas = Resources.Load<NGUIAtlas>("Atlas/login");
        sprite.atlas = atlas;
        //再设置图片
        sprite.spriteName = "ui_DL_anniuxiao_01";
    }
}
```