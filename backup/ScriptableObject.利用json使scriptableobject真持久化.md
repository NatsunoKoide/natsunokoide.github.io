### 利用json实现持久化
**通过在继承scriptableobject父类的数据脚本中，写（1）判断文件是否存在 若存在直接使用 （2）写一个保存函数 用于持久化数据 **

### （1）判断文件是否存在 若存在直接使用
```js
    private void Awake()
    {
        //在awake函数里判断一下硬盘里是否有这个数据
        if (File.Exists(Application.persistentDataPath + "/SettingInfo.json"))
        {
            string str = File.ReadAllText(Application.persistentDataPath + "/SettingInfo.json");
            JsonUtility.FromJsonOverwrite(str, this);
        }
    }
```

### （2）写一个保存函数 用于持久化数据 
```js
    /// <summary>
    /// 保存到本地持久化
    /// </summary>
    public void Save()
    {
        string str = JsonUtility.ToJson(this);
        File.WriteAllText(Application.persistentDataPath + "/SettingInfo.json", str);
    }
```