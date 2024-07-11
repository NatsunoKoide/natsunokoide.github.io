### LuaMgr——版本一
### 内容
**主要用于封装xlua的文件执行垃圾回收销毁
   关键内容 在于 对于本地文件的lua读取以及对于ab包内的lua读取 （重定向）
   提供可以获取_G表中 的全部lua全局变量 的属性**

### 代码
```js
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using XLua;


/// <summary>
/// Lua管理器
/// </summary>
public class LuaMgr : BaseManager<LuaMgr>
{
    private LuaEnv luaEnv;

    public LuaTable Global
    {
        get
        {
            return luaEnv.Global;
        }
    }

    private void Init()
    {
        //已经初始化 就直接返回
        if (luaEnv != null) return;
        //初始化
        luaEnv = new LuaEnv();
        //加载脚本 重定向
        luaEnv.AddLoader(MyCustomLoader);
        luaEnv.AddLoader(MyCustomABLoader);
    }

    /// <summary>
    /// DoString函数的简化版本
    /// </summary>
    /// <param name="fileName"></param>
    public void DoLuaFile(string fileName)
    {
        string str = string.Format("require('{0}')", fileName);
        DoString(str);
    }

    /// <summary>
    /// 执行lua语言
    /// </summary>
    /// <param name="str"></param>
    public void DoString(string str)
    {
        luaEnv.DoString(str);
    }

    /// <summary>
    /// 释放lua 垃圾
    /// </summary>
    public void Tick()
    {
        luaEnv.Tick();
    }

    /// <summary>
    /// 销毁解析器
    /// </summary>
    public void Dispose()
    {
        luaEnv.Dispose();
        luaEnv = null;
    }

    private byte[] MyCustomLoader(ref string filePath)
    {
        //Debug.Log(filePath);

        //传入的参数是 require 执行的lua的脚本文件名
        //拼接一个lua文件的所在路径
        string path = Application.dataPath + "/Lua/" + filePath + ".lua";
        Debug.Log(path);
        //有路径就加载文件
        //file 只是点 c#提供的文件读写类
        if (File.Exists(path))
        {
            return File.ReadAllBytes(path);
        }
        else
        {
            Debug.Log("重定向失败，文件名为" + filePath);
        }

        return null;
    }

    private byte[] MyCustomABLoader(ref string filePath)
    {
        #region ab包内加载lua文件（不使用ab管理器）
        //Debug.Log("进入AB包加载 重定向函数");
        ////从AB包中加载lua文件
        ////加载AB包
        //string path = Application.streamingAssetsPath + "/lua";
        //AssetBundle ab = AssetBundle.LoadFromFile(path);

        ////加载Lua文件 返回
        //TextAsset tx = ab.LoadAsset<TextAsset>(filePath + ".lua");
        ////加载Lua文件 byte数组
        //return tx.bytes;
        #endregion

        #region  ab包内加载lua文件（使用ab管理器）
        TextAsset lua = ABMgr.GetInstance().LoadRes<TextAsset>("lua", filePath + ".lua");
        if (lua != null)
            return lua.bytes;
        else
            Debug.Log("MyCustomABLoader重定向失败，文件名为：" + filePath);
        return null;
        #endregion
    }
}
```