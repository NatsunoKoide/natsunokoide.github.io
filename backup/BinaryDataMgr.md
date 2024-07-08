### 工具相关说明
**
1.Excel文件应该放置在ArtRes/Excel当中
如果想要修改 就去修改ExcelTool当中的EXCEL_PATH路径变量
2.配置表规则
	第一行：字段名
	第二行：字段类型（字段类型一定不要配置错误，字段类型目前只支持int float bool string）
	如果想要再添加类型，需要在ExcelTool的GenerateExcelBinary方法中
	和BinaryDataMgr的LoadTable方法当中对应添加读写的逻辑
	第三行：主键是哪一个字段 需要通过key来标识主键
	第四行：描述信息（只是给别人看，不会有别的作用）
	第五行~第n行：就是具体数据信息
	下方的表名决定了数据结构类，容器类，2进制文件的文件名
3.生成的容器类和数据结构类可以在ExcelTool当中修改DATA_CLASS_PATH和DATA_CONTAINER_PATH
变量来进行更改
4.生成2进制文件的路径可以在BinaryDataMgr当中的DATA_BINARY_PATH变量来进行修改
**

### 脚本说明
**
ExcelTool：用于设置unity 的编辑器功能 和excel的读取写入 以及 excel文件摆放位置
BinaryDataMgr：实现容器类 数据结构类 以及二进制表的读取
**

### 代码
### ExcelTool
```js
using Excel;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Text;
using UnityEditor;
using UnityEngine;

public class ExcelTool
{
    /// <summary>
    /// excel文件存放的路径
    /// </summary>
    public static string EXCEL_PATH = Application.dataPath + "/ArtRes/Excel/";

    /// <summary>
    /// 数据结构类脚本存储位置路径
    /// </summary>
    public static string DATA_CLASS_PATH = Application.dataPath + "/Scripts/ExcelData/DataClass/";

    /// <summary>
    /// 容器类脚本存储位置路径
    /// </summary>
    public static string DATA_CONTAINER_PATH = Application.dataPath + "/Scripts/ExcelData/Container/";

    /// <summary>
    /// 真正内容开始的行号
    /// </summary>
    public static int BEGIN_INDEX = 4;

    [MenuItem("GameTool/GenerateExcel")]
    private static void GenerateExcelInfo()
    {
        //记在指定路径中的所有Excel文件 用于生成对应的3个文件
        DirectoryInfo dInfo = Directory.CreateDirectory(EXCEL_PATH);
        //得到指定路径中的所有文件信息 相当于就是得到所有的Excel表
        FileInfo[] files = dInfo.GetFiles();
        //数据表容器
        DataTableCollection tableConllection;
        for (int i = 0; i < files.Length; i++)
        {
            //如果不是excel文件就不要处理了
            if (files[i].Extension != ".xlsx" &&
                files[i].Extension != ".xls")
                continue;
            //打开一个Excel文件得到其中的所有表的数据
            using (FileStream fs = files[i].Open(FileMode.Open, FileAccess.Read))
            {
                IExcelDataReader excelReader = ExcelReaderFactory.CreateOpenXmlReader(fs);
                tableConllection = excelReader.AsDataSet().Tables;
                fs.Close();
            }

            //遍历文件中的所有表的信息
            foreach (DataTable table in tableConllection)
            {
                //生成数据结构类
                GenerateExcelDataClass(table);
                //生成容器类
                GenerateExcelContainer(table);
                //生成2进制数据
                GenerateExcelBinary(table);
            }

        }
    }

    /// <summary>
    /// 生成Excel表对应的数据结构类
    /// </summary>
    /// <param name="table"></param>
    private static void GenerateExcelDataClass(DataTable table)
    {
        //字段名行
        DataRow rowName = GetVariableNameRow(table);
        //字段类型行
        DataRow rowType = GetVariableTypeRow(table);

        //判断路径是否存在 没有的话 就创建文件夹
        if (!Directory.Exists(DATA_CLASS_PATH))
            Directory.CreateDirectory(DATA_CLASS_PATH);
        //如果我们要生成对应的数据结构类脚本 其实就是通过代码进行字符串拼接 然后存进文件就行了
        string str = "public class " + table.TableName + "\n{\n";

        //变量进行字符串拼接
        for (int i = 0; i < table.Columns.Count; i++)
        {
            str += "    public " + rowType[i].ToString() + " " + rowName[i].ToString() + ";\n";
        }

        str += "}";

        //把拼接好的字符串存到指定文件中去
        File.WriteAllText(DATA_CLASS_PATH + table.TableName + ".cs", str);

        //刷新Project窗口
        AssetDatabase.Refresh();
    }

    /// <summary>
    /// 生成Excel表对应的数据容器类
    /// </summary>
    /// <param name="table"></param>
    private static void GenerateExcelContainer(DataTable table)
    {
        //得到主键索引
        int keyIndex = GetKeyIndex(table);
        //得到字段类型行
        DataRow rowType = GetVariableTypeRow(table);
        //没有路径创建路径
        if (!Directory.Exists(DATA_CONTAINER_PATH))
            Directory.CreateDirectory(DATA_CONTAINER_PATH);

        string str = "using System.Collections.Generic;\n";

        str += "public class " + table.TableName + "Container" + "\n{\n";

        str += "    ";
        str += "public Dictionary<" + rowType[keyIndex].ToString() + ", " + table.TableName + ">";
        str += "dataDic = new " + "Dictionary<" + rowType[keyIndex].ToString() + ", " + table.TableName + ">();\n";

        str += "}";

        File.WriteAllText(DATA_CONTAINER_PATH + table.TableName + "Container.cs", str);

        //刷新Project窗口
        AssetDatabase.Refresh();
    }


    /// <summary>
    /// 生成excel2进制数据
    /// </summary>
    /// <param name="table"></param>
    private static void GenerateExcelBinary(DataTable table)
    {
        //没有路径创建路径
        if (!Directory.Exists(BinaryDataMgr.DATA_BINARY_PATH))
            Directory.CreateDirectory(BinaryDataMgr.DATA_BINARY_PATH);

        //创建一个2进制文件进行写入
        using (FileStream fs = new FileStream(BinaryDataMgr.DATA_BINARY_PATH + table.TableName + ".tang", FileMode.OpenOrCreate, FileAccess.Write))
        {
            //存储具体的excel对应的2进制信息
            //1.先要存储我们需要写多少行的数据 方便我们读取
            //-4的原因是因为 前面4行是配置规则 并不是我们需要记录的数据内容
            fs.Write(BitConverter.GetBytes(table.Rows.Count - 4), 0, 4);
            //2.存储主键的变量名
            string keyName = GetVariableNameRow(table)[GetKeyIndex(table)].ToString();
            byte[] bytes = Encoding.UTF8.GetBytes(keyName);
            //存储字符串字节数组的长度
            fs.Write(BitConverter.GetBytes(bytes.Length), 0, 4);
            //存储字符串字节数组
            fs.Write(bytes, 0, bytes.Length);

            //遍历所有内容的行 进行2进制的写入
            DataRow row;
            //得到类型行 根据类型来决定应该如何写入数据
            DataRow rowType = GetVariableTypeRow(table);
            for (int i = BEGIN_INDEX; i < table.Rows.Count; i++)
            {
                //得到一行的数据
                row = table.Rows[i];
                for (int j = 0; j < table.Columns.Count; j++)
                {
                    switch (rowType[j].ToString())
                    {
                        case "int":
                            fs.Write(BitConverter.GetBytes(int.Parse(row[j].ToString())), 0, 4);
                            break;
                        case "float":
                            fs.Write(BitConverter.GetBytes(float.Parse(row[j].ToString())), 0, 4);
                            break;
                        case "bool":
                            fs.Write(BitConverter.GetBytes(bool.Parse(row[j].ToString())), 0, 1);
                            break;
                        case "string":
                            bytes = Encoding.UTF8.GetBytes(row[j].ToString());
                            //写入字符串字节数组的长度
                            fs.Write(BitConverter.GetBytes(bytes.Length), 0, 4);
                            //写入字符串字节数组
                            fs.Write(bytes, 0, bytes.Length);
                            break;
                    }
                }
            }

            fs.Close();
        }

        AssetDatabase.Refresh();
    }

    /// <summary>
    /// 获取变量名所在行
    /// </summary>
    /// <param name="table"></param>
    /// <returns></returns>
    private static DataRow GetVariableNameRow(DataTable table)
    {
        return table.Rows[0];
    }

    /// <summary>
    /// 获取变量类型所在行
    /// </summary>
    /// <param name="table"></param>
    /// <returns></returns>
    private static DataRow GetVariableTypeRow(DataTable table)
    {
        return table.Rows[1];
    }

    
    /// <summary>
    /// 获取主键索引
    /// </summary>
    /// <param name="table"></param>
    /// <returns></returns>
    private static int GetKeyIndex(DataTable table)
    {
        DataRow row = table.Rows[2];
        for (int i = 0; i < table.Columns.Count; i++)
        {
            if (row[i].ToString() == "key")
                return i;
        }
        return 0;
    }
}

```

### BinaryDataMgr
```js
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Reflection;
using System.Runtime.Serialization.Formatters.Binary;
using System.Text;
using UnityEngine;

/// <summary>
/// 2进制数据管理器
/// </summary>
public class BinaryDataMgr
{
    /// <summary>
    /// 2进制数据存储位置路径
    /// </summary>
    public static string DATA_BINARY_PATH = Application.streamingAssetsPath + "/Binary/";

    /// <summary>
    /// 用于存储所有Excel表数据的容器
    /// </summary>
    private Dictionary<string, object> tableDic = new Dictionary<string, object>();

    /// <summary>
    /// 数据存储的位置
    /// </summary>
    private static string SAVE_PATH = Application.persistentDataPath + "/Data/";

    private static BinaryDataMgr instance = new BinaryDataMgr();
    public static BinaryDataMgr Instance => instance;

    private BinaryDataMgr()
    {
        InitData();
    }

    public void InitData()
    {
        
    }

    /// <summary>
    /// 加载Excel表的2进制数据到内存中 
    /// </summary>
    /// <typeparam name="T">容器类名</typeparam>
    /// <typeparam name="K">数据结构类类名</typeparam>
    public void LoadTable<T,K>()
    {
        //读取 excel表对应的2进制文件 来进行解析
        using (FileStream fs = File.Open(DATA_BINARY_PATH + typeof(K).Name + ".tang", FileMode.Open, FileAccess.Read))
        {
            byte[] bytes = new byte[fs.Length];
            fs.Read(bytes, 0, bytes.Length);
            fs.Close();
            //用于记录当前读取了多少字节了
            int index = 0;

            //读取多少行数据
            int count = BitConverter.ToInt32(bytes, index);
            index += 4;

            //读取主键的名字
            int keyNameLength = BitConverter.ToInt32(bytes, index);
            index += 4;
            string keyName = Encoding.UTF8.GetString(bytes, index, keyNameLength);
            index += keyNameLength;

            //创建容器类对象
            Type contaninerType = typeof(T);
            object contaninerObj = Activator.CreateInstance(contaninerType);
            //得到数据结构类的Type
            Type classType = typeof(K);
            //通过反射 得到数据结构类 所有字段的信息
            FieldInfo[] infos = classType.GetFields();

            //读取每一行的信息
            for (int i = 0; i < count; i++)
            {
                //实例化一个数据结构类 对象
                object dataObj = Activator.CreateInstance(classType);
                foreach (FieldInfo info in infos)
                {
                    if( info.FieldType == typeof(int) )
                    {
                        //相当于就是把2进制数据转为int 然后赋值给了对应的字段
                        info.SetValue(dataObj, BitConverter.ToInt32(bytes, index));
                        index += 4;
                    }
                    else if (info.FieldType == typeof(float))
                    {
                        info.SetValue(dataObj, BitConverter.ToSingle(bytes, index));
                        index += 4;
                    }
                    else if (info.FieldType == typeof(bool))
                    {
                        info.SetValue(dataObj, BitConverter.ToBoolean(bytes, index));
                        index += 1;
                    }
                    else if (info.FieldType == typeof(string))
                    {
                        //读取字符串字节数组的长度
                        int length = BitConverter.ToInt32(bytes, index);
                        index += 4;
                        info.SetValue(dataObj, Encoding.UTF8.GetString(bytes, index, length));
                        index += length;
                    }
                }

                //读取完一行的数据了 应该把这个数据添加到容器对象中
                //得到容器对象中的 字典对象
                object dicObject = contaninerType.GetField("dataDic").GetValue(contaninerObj);
                //通过字典对象得到其中的 Add方法
                MethodInfo mInfo = dicObject.GetType().GetMethod("Add");
                //得到数据结构类对象中 指定主键字段的值
                object keyValue = classType.GetField(keyName).GetValue(dataObj);
                mInfo.Invoke(dicObject, new object[] { keyValue, dataObj });
            }

            //把读取完的表记录下来
            tableDic.Add(typeof(T).Name, contaninerObj);

            fs.Close();
        }
    }

    /// <summary>
    /// 得到一张表的信息
    /// </summary>
    /// <typeparam name="T">容器类名</typeparam>
    /// <returns></returns>
    public T GetTable<T>() where T:class
    {
        string tableName = typeof(T).Name;
        if (tableDic.ContainsKey(tableName))
            return tableDic[tableName] as T;
        return null;
    }

    /// <summary>
    /// 存储类对象数据
    /// </summary>
    /// <param name="obj"></param>
    /// <param name="fileName"></param>
    public void Save(object obj, string fileName)
    {
        //先判断路径文件夹有没有
        if (!Directory.Exists(SAVE_PATH))
            Directory.CreateDirectory(SAVE_PATH);

        using (FileStream fs = new FileStream(SAVE_PATH + fileName + ".tang", FileMode.OpenOrCreate, FileAccess.Write))
        {
            BinaryFormatter bf = new BinaryFormatter();
            bf.Serialize(fs, obj);
            fs.Close();
        }
    }

    /// <summary>
    /// 读取2进制数据转换成对象
    /// </summary>
    /// <typeparam name="T"></typeparam>
    /// <param name="fileName"></param>
    /// <returns></returns>
    public T Load<T>(string fileName) where T:class
    {
        //如果不存在这个文件 就直接返回泛型对象的默认值
        if( !File.Exists(SAVE_PATH + fileName + ".tang") )
            return default(T);

        T obj;
        using (FileStream fs = File.Open(SAVE_PATH + fileName + ".tang", FileMode.Open, FileAccess.Read))
        {
            BinaryFormatter bf = new BinaryFormatter();
            obj = bf.Deserialize(fs) as T;
            fs.Close();
        }

        return obj;
    }
}
```