### 知识点
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