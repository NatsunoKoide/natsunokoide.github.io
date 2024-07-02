```js
using Excel;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.IO;
using UnityEditor;
using UnityEngine;

public class LessonExcelRead
{
    #region 知识点一  打开Excel表
    //主要知识点：
    //1.FileStream 文件流
    //2.IExccelDataReader读取Excel数据
    //3.DataSet 数据集合类 将Excel中的数据转存进去

    [MenuItem("GameTool/打开Excel表")]
    private static void OpenExcel()
    {
        using (FileStream fs = File.Open(Application.dataPath + "/ArtRes/Excel/PlayerInfo.xlsx",FileMode.Open,FileAccess.Read))
        {
            //利用Excel工具类中的方法读取文件流获取Excel数据
            IExcelDataReader excelReader = ExcelReaderFactory.CreateOpenXmlReader(fs);
            //将excel表中数据转换为DataSet数据类型
            DataSet result = excelReader.AsDataSet();
            for(int i = 0;i < result.Tables.Count;i++)
            {
                Debug.Log("表名 " + result.Tables[i].TableName);
                Debug.Log("行数 " + result.Tables[i].Rows.Count);
                Debug.Log("列数 " + result.Tables[i].Columns.Count);
            }
            fs.Close();
        }
    }
    #endregion

    #region 知识点二 获取单元格信息
    private static void ReadExcel()
    {
        using (FileStream fs = File.Open(Application.dataPath + "/ArtRes/Excel/PlayerInfo.xlsx", FileMode.Open, FileAccess.Read))
        {
            IExcelDataReader excelReader = ExcelReaderFactory.CreateOpenXmlReader(fs);
            DataSet result = excelReader.AsDataSet();
            //取excel数据出来
            for (int i = 0; i < result.Tables.Count; i++)
            {
                //得到其中一个表的具体数据
                DataTable table = result.Tables[i];
                /*//得到其中一行的数据
                DataRow row = tabel.Rows[0];
                //得到行中某一列的信息
                Debug.Log(row[1].ToString());*/
                DataRow row;
                for (int j = 0; j < table.Rows.Count; j++)
                {
                    //获取到其中一行
                    row = table.Rows[j];
                    Debug.Log("**********************新的一行**************************");
                    for (int k = 0; k < table.Columns.Count; k++)
                    {
                        //打印当前行的所有信息
                        Debug.Log(row[k].ToString());
                    }
                }
            }
        }
    }
    #endregion
}
```