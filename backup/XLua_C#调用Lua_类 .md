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

### 知识点5 —— ref和out的调用
1.C#中的类+方法声明
```js
public class Lesson5
{
    public int RefFun(int a,ref int b,ref int c,int d)
    {
        b = a + d;
        c = a - d;
        return 100;
    }

    public int OutFun(int a, out int b, out int c, int d)
    {
        b = a;
        c = d;
        return 200;
    }

    public int RefOutFun(int a, out int b, ref int c)
    {
        b = a * 10;
        c = a * 20;
        return 200;
    }
}
```
2.lua脚本
```js
print("*********Lua调用c# refout相关知识点**********")

Lesson5 = CS.Lesson5

local obj = Lesson5()

--ref参数 会以多返回值的形式返回给lua
--如果函数存在返回值 那么第一个值 就是该返回值
--之后的返回值就说ref的结果 从左到右一一对应
--ref参数 需要传入一个默认值 占位置
--a 相当于 默认返回值 b 相当于第一个 ref  c 相当于第二个ref

local a,b,c = obj:RefFun(1,0,0,1)
print(a);
print(b);
print(c);

--ref参数 会以多返回值的形式返回给lua
--如果函数存在返回值 那么第一个值 就是该返回值 
--之后的返回值就说ref的结果 从左到右一一对应
--ref参数 不需要传入一个默认值 占位置 （（（（（（（（（（这是个ref的区别））））））））））
local a,b,c = obj:OutFun(20,30)
print(a);
print(b);
print(c);

--第三种情况混合ref和out
local a,b,c = obj:RefOutFun(20,1)
print(a);
print(b);
print(c);
```

### 知识点6 —— 重载函数
1.c#中的类
```js
public class Lesson6
{
    public int Calc()
    {
        return 100;
    }
    public int Calc(int a,int b)
    {
        return a + b;
    }
    public int Calc(int a)
    {
        return a;
    }
    public float Calc(float a)
    {
        return a;
    }
}
```

2.lua脚本
```js
print("*****************lua调用c# 重载函数知识点*******************")

local obj = CS.Lesson6()
--LUA自己不支持重载函数 但是lua支持调用c#的重载

print(obj:Calc())
print(obj:Calc(15,1))
--lua虽然支持重载c#
--但是lua的数值类型只有number
--对c#多精度的重载支持不是很好
--使用时会出现意料之外的返回值
--所以尽可能避免使用不同精度类型的重载
print(obj:Calc(10))
--此处无法正常输出float类型
print(obj:Calc(10.2))

--针对重载含糊问题的解决办法（只做了解不会实际开发不会采用——效率很低）
--利用typeof反射关键类
local m1 = typeof(CS.Lesson6):GetMethod("Calc",{typeof(CS.System.Int32)})
local m2 = typeof(CS.Lesson6):GetMethod("Calc",{typeof(CS.System.Single)})

--通过xlua提供的方法 把函数转为lua函数使用
--一般转一次 然后重复使用
local f1 = xlua.tofunction(m1)
local f2 = xlua.tofunction(m2)

--调用 转换之后的函数方法 第一个参数为类对象 第二个参数是输入参数
print(f1(obj,10))
print(f2(obj,10.2))
```

### 知识点7 —— 委托和事件 
1.c#内容
```js
public class Lesson7
{
    //申明委托
    public UnityAction del;

    //申明事件(事件只能在类内部调用)
    public event UnityAction eventAction;

    public void DoEvent()
    {
        if(eventAction != null)
        {
            eventAction();
        }
    }

    public void ClearEvent()
    {
        eventAction = null;
    }
}
#endregion
```
2.lua函数
```js
print("*****************lua调用c#委托事件知识点*******************")

local obj = CS.Lesson7()

print("——————————————————————————委托相关————————————————————")
--委托是用来装函数的 使用c#的委托 就是用来装lua函数
local fun = function()
	print("lua函数Fun")
end

--lua 没有复合运算符 不能+=
--如果第一次往委托中加函数 因为是nil 不能直接+

--委托第一次直接等于就行(很关键)

print("——————————————————————————开始加函数————————————————————")
obj.del = fun
obj.del = obj.del + fun
--最好不要这样写 有点匿名函数的感觉 不好去除
obj.del = obj.del + function()
	print("临时定义的函数")
end
--委托执行
obj.del()
print("——————————————————————————开始减函数————————————————————")
obj.del = obj.del - fun
obj.del = obj.del - fun
--委托执行
obj.del()
--委托清空函数
obj.del = nil
--清空后先等于
obj.del = fun
--委托调用
obj.del()

print("——————————————————————————事件相关————————————————————")
local fun2 = function()
	print("事件加的函数")
end
print("——————————————————————————加事件函数————————————————————")
--事件加减函数 和 委托非常不一样
--lua使用c#事件 加函数 
--有点类似于使用成员方法 冒号事件名（"+",函数变量）
--最好不要用类似匿名函数的写法 
obj:eventAction("+",fun2)
obj:eventAction("+",fun2)
--事件执行
obj:DoEvent()
print("——————————————————————————减事件函数————————————————————")
obj:eventAction("-",fun2)
obj:DoEvent()
print("——————————————————————————事件清除————————————————————")
--注意 事件不能像委托一样 置空清空 
--可以在c#中对事件写一个清空函数 在lua中调用
obj:ClearEvent()
obj:DoEvent()
```
3.区分函数和事件
区别：
事件的执行可能涉及多个线程，而函数是单线程的
每个事件触发以后，系统就开一个线程执行这个事件。所以可以使用Delay
而由于函数是单线程的，调用函数以后该线程就在等待函数的执行直到其返回，函数内部无法停等，即函数中不能使用Timeline, Delay。
事件没有返回值
事件的意义是游戏中发生的事情。如玩家按下前进键。
适用：
需要停等如Delay的时候使用Event
需要返回值的时候使用Function
由游戏中的“事件”触发的逻辑应该使用Event来写
由于事件的多线程特性，其执行顺序是不定的。所以如果想封装一个不需要返回值，不需要停等，不由游戏中“事件”触发 的功能，应尽可能使用Function，顺序比较可控
原文链接：https://blog.csdn.net/weixin_44559752/article/details/123027442