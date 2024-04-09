### SciptabelObject.数据文件创建

### 方法一 —— 特性一 —— [CreateAssetMenu(fileName = "MyData",menuName = "我的数据",order = 0)]
```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Video;

[CreateAssetMenu(fileName = "MyData",menuName = "我的数据",order = 0)]
public class MyData : ScriptableObject
{
    public int i;
    public float f;
    public bool b;

    public GameObject obj;
    public Material m;
    public AudioClip audioClip;
    public VideoClip videoClip;
}
```

### 方法二 —— 特性二 —— [MenuItem("ScriptableObject/CreateMyData")]
```js
using System.Collections;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

public class ScriptableObjectTool : MonoBehaviour
{
    [MenuItem("ScriptableObject/CreateMyData")]
    public static void CreateMyData()
    {
        //书写创建数据资源文件的代码
        MyData asset = ScriptableObject.CreateInstance<MyData>();
        //通过编辑器api 创建一个数据资源文件
        AssetDatabase.CreateAsset(asset, "Assets/Resources/MyDataTest.asset");
        //保存创建的资源
        AssetDatabase.SaveAssets();
        //刷新一次界面
        AssetDatabase.Refresh();
    }
}
```