### RectTransformUtility类 用于转换ui坐标的类
### RectTransformUtility.ScreenPointToLocalPointInRectangle的使用

> 屏幕点转当前坐标点

```js
    private Vector2 nowPos;
    public void OnDrag(PointerEventData eventData)
    {
        RectTransformUtility.ScreenPointToLocalPointInRectangle(
            //相对父对象
            this.transform.parent as RectTransform,
            //屏幕点
            eventData.position,
            //摄像机
            eventData.enterEventCamera,
            //目标点
            out nowPos
            );
        //通过RectTransformUtility.ScreenPointToLocalPointInRectangle函数out出来的目标点
        //将目标点赋值给挂载对象即可实现坐标转换
        this.transform.localPosition = nowPos;
        //上方这种方式 用于优化下方代码 
        //this.transform.position += new Vector3(eventData.delta.x, eventData.delta.y, 0);
    }
```