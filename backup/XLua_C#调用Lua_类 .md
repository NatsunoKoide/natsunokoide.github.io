### 知识点1 —— 类
1.  Lua调用C#类                          CS.命名空间.类名        CS.UnityEngine.类名
2. 没有命名空间的自定义类        CS.类名
3. 实例化对象                              CS.命名空间.类名
4. 静态方法和变量                       CS.命名空间.类名.方法
5. 成员变量                                  实例化对象.变量名
6. 成员方法                                  实例化对象：方法名
7. 技巧点                                      
（1）用别名定义  ***=CS.命名空间.类名 之后可以直接使用***代替 
（2）xlua不支持无参泛型很多时候需要使用含有typeof的重载 
（3）一定要通过c#启动lua脚本 才能实现交互

### 代码
```js
print("*********Lua调用c#相关知识点**********")

local obj1 = CS.UnityEngine.GameObject()
local obj2 = CS.UnityEngine.GameObject("123")

GameObject = CS.UnityEngine.GameObject
local obj3 = GameObject("测试对象")

local t = CS.Test()
t:Speak("testspeak")

local t2 = CS.Finn.Test2()
t2:Speak("test2speak")

local obj4 = GameObject("加脚本测试")
obj4:AddComponent(typeof(CS.LuaCallCSharp))
```

### 知识点2 —— 枚举
1.枚举的使用与类相似，无法实例化 直接等于号即可
CS.命名空间.枚举名.枚举成员        /        枚举.__CastFrom(数字或者字符串)

### 知识点3 —— List/Dic
```js
print("*********Lua调用c# list dictionary相关知识点**********")

--直接实例化包含数组的类
local obj = CS.Lesson3()

--Lua使用c#数组 
--c#怎么使用 lua就怎么使用 不能使用#获取长度
--回忆：lua 可以使用 #obj.Array获得长度
print(obj.array.Length)

--访问元素
print(obj.array[0])

--遍历 按c#规则从0 开始 且结束需要 -1 lua遍历默认最后带等于
for i = 0, obj.array.Length - 1 do
	print(obj.array[i])
end

--lua种创建一个c#数组lua种表示数组和list可以使用表
--创建c#种的数字 使用array类中的静态方法
local array2 = CS.System.Array.CreateInstance(typeof(CS.System.Int32),10)
print(array2.Length)
print(array2[0])

print("*********Lua调用c# list相关知识点**********")
obj.list:Add(1)
obj.list:Add(2)
obj.list:Add(3)
--长度
print(obj.list.Count)
--遍历
for i=0,obj.list.Count - 1 do
	print(obj.list[i])
end
print(obj.list)

--lua中创建一个list对象
--老版本
local list2 = CS.System.Collections.Generic["List`1[System.String]"]()
print(list2)
--新版本 > v2.1.12
--申明一个泛型为string的list类结构
local list_String = CS.System.Collections.Generic.List(CS.System.String)
--实例化出来
local list3 = list_String()
list3:Add("2222")
print(list3[0])

print("*********Lua调用c# dic相关知识点**********")
--使用
obj.dic:Add(1,"123")
print(obj.dic[1])

--遍历
for k,v in pairs(obj.dic) do
	print(k,v)
end

--在lua创建一个dic 
--相当于得到一个dictionary 的一个类别名，需要再实例化
local Dic_String_Vector3 = CS.System.Collections.Generic.Dictionary(CS.System.String,CS.UnityEngine.Vector3)
local dic2 = Dic_String_Vector3()
dic2:Add("123",CS.UnityEngine.Vector3.right)
for i,v in pairs(dic2) do
	print(i,v)
end
--尝试获取值 返回 bool 和 值
print(dic2:TryGetValue("123"))
--在lua创建的字典 通过括号只能得到nil
print(dic2["123"])
--如果通过键来获得值 需要采用固定方法如下
print(dic2:get_Item("123"))
--设置键值对也可以采用set如下
dic2:set_Item("123",nil)
print(dic2:get_Item("123"))
```
### 知识点4 —— 拓展方法的使用
1. c#中代码
```js
//若要在lua中使用工具类中的拓展方法 要加特性
//建议lua使用了的类 都加上特性 可以增加性能
[LuaCallCSharp]
public static class Tools
{
    //lesson4的拓展方法的写法
    public static void Move(this Lesson4 obj)
    {
        Debug.Log(obj.name + "移动");
    }
}
public class Lesson4
{
    public string name = "123";
    public void Speak(string str)
    {
        Debug.Log(str);
    }

    public static void Eat()
    {
        Debug.Log("吃东西");
    }
}
```
2. lua脚本
```js
print("*********Lua调用c# 拓展方法相关知识点**********")

Lesson4 = CS.Lesson4
--使用静态方法
--CS.命名空间.类名.静态方法（）
Lesson4.Eat()

--成员方法  实例化出来用 用 ： 
local obj = Lesson4()
obj:Speak("123")

--使用拓展方法 和使用成员方法一致
--要调用 c# 的某个类的拓展方法  那么一定要 在拓展方法的
obj:Move()
```