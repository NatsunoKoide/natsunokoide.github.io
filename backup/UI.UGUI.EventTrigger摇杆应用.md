### EventTrigger摇杆应用
**本文使用EventTrigger实现手机游戏的摇杆控制玩家移动的功能。**

### 面板代码部分
**关于EventTrigger在start函数中的注册**

> 函数定义

```js
    //摇杆内容
    //RectTransform是定义ui位置的类
    public RectTransform imgJoy;
    public EventTrigger et;
```

> 注册代码

```js
        //为摇杆在事件触发器上注册事件
        //创建新的事件
        EventTrigger.Entry en = new EventTrigger.Entry();
        //定义新事件的类型
        en.eventID = EventTriggerType.Drag;
        //给出这一新事件类型要做的事情（函数）
        en.callback.AddListener(JoyDrag);
        //在事件触发器的triggers列表中加入新事件
        et.triggers.Add(en);

        //创建第二个（只不过可以用同一个名字加入）
        en = new EventTrigger.Entry();
        //定义新事件的类型
        en.eventID = EventTriggerType.EndDrag;
        //给出这一新事件类型要做的事情（函数）
        en.callback.AddListener(EndJoyDrag);
        //在事件触发器的triggers列表中加入新事件
        et.triggers.Add(en);
```

> 摇杆拖拽事件以及停止拖曳 

> 其中涉及到的move函数在player脚本中 用于2dui和3d物体的坐标转换从而使玩家可以在3d空间正常移动

```js
    private void JoyDrag(BaseEventData data)
    {
        //获取data中的内容 （很重要） 里氏替换原则 
        PointerEventData eventData = data as PointerEventData;
        imgJoy.position += new Vector3(eventData.delta.x, eventData.delta.y, 0);
        if(imgJoy.anchoredPosition.magnitude > 170)
        {
            imgJoy.anchoredPosition = imgJoy.anchoredPosition.normalized * 170;
        }

        Player.Move(imgJoy.anchoredPosition);
    }

    private void EndJoyDrag(BaseEventData data)
    {
        //回归原点
        imgJoy.anchoredPosition = Vector2.zero;
        //停止移动
        Player.Move(Vector2.zero);
    }
```

### 玩家脚本内容

> 参数定义

```js
  //玩家在3d空间 所以定义vector3
    private Vector3 nowMoveDir = Vector3.zero;
    //移动速度
    public float moveSpeed = 10;
    //转向速度
    public float rotateSpeed = 10;
```

> update函数（用于判定玩家的移动）
```js
void Update()
    {
        if(nowMoveDir != Vector3.zero)
        {
            this.transform.Translate(Vector3.forward * Time.deltaTime * moveSpeed);
            this.transform.rotation = Quaternion.Slerp(this.transform.rotation, Quaternion.LookRotation(nowMoveDir), Time.deltaTime * rotateSpeed);
        }
    }
```
> move函数用于转换坐标
```js
    public void Move(Vector2 dic)
    {
        nowMoveDir.x = dic.x;
        nowMoveDir.z = dic.y;
        nowMoveDir.y = 0;
    }
```