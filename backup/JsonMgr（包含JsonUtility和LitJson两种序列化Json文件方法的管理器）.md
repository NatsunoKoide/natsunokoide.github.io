> 管理器代码
```js
using LitJson;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;

public enum JsonType
{
    JsonUtlity,
    LitJson,
}

/// <summary>
/// Json数据管理类 主要要能甘于 Json序列化存储到硬盘 和 反序列化从硬盘中读取到内存中
/// </summary>
public class JsonMgr
{
    //不继承mono就new一个instance出来 如果继承就在awake函数里赋予
    private static JsonMgr instance = new JsonMgr();
    public static JsonMgr Instacne => instance;

    private JsonMgr() { }

    //存储Json数据 序列化
    public void SaveData(object data,string fileName,JsonType type = JsonType.LitJson)
    {
        //确定存储路径
        string path = Application.persistentDataPath + "/" + fileName + ".json";
        string jsonStr = "";
        switch (type)
        {
            case JsonType.JsonUtlity:
                jsonStr = JsonUtility.ToJson(data);
                break;
            case JsonType.LitJson:
                jsonStr = JsonMapper.ToJson(data);
                break;
        }
        //把序列化的Json字符串 存储到指定路径的文件中
        File.WriteAllText(path, jsonStr);
    }

    //where : new() 泛型约束 ： 表示类型参数T必须具有一个公开的无参数的构造函数
    public T LoadData<T>(string fileName,JsonType type = JsonType.LitJson) where T : new()
    {
        //判断默认数据文件中是否有想要的数据 如果有从中获取
        string path = Application.streamingAssetsPath + "/" + fileName + ".json";
        if(!File.Exists(path)) path = Application.persistentDataPath + "/" + fileName + ".json";
        if(!File.Exists(path)) return new T(); // 因为有 where T : new() 约束，这里可以安全地创建T类型的实例
        //泛型约束是用来限制泛型类型参数可以代表的数据类型范围的。
        //where T : new()就是这样一种约束，确保了你可以安全地使用new T()语法来实例化T类型的对象。
        //如果没有这个约束，尝试实例化T时编译器将报错，因为T可能没有公开的无参数构造函数。

        //读取文件信息
        string jsonStr = File.ReadAllText(path);
        //数据对象申明
        T data = default(T);
        //反序列化
        switch (type)
        {
            case JsonType.JsonUtlity:
                data = JsonUtility.FromJson<T>(jsonStr);
                break;
            case JsonType.LitJson:
                data = JsonMapper.ToObject<T>(jsonStr);
                break;
        }

        //default(T) 泛型返回占用
        return data;
    }
}

```