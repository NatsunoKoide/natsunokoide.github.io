### LineRender画圆函数
```js
public void DrawLineRenderer(Vector3 centerPos, float r, int pointNum)
    {
        //动态创建 画线对象
        GameObject obj = new GameObject();
        obj.name = "R";
        LineRenderer line = obj.AddComponent<LineRenderer>();
        line.loop = false;
        //设置有多少个点
        line.positionCount = pointNum;
        //让其首尾相连
        line.loop = true;

        //得到每个点之间 间隔的度数
        float angle = 360f / pointNum;

        //准备得到每一个点
        for (int i = 0; i < pointNum; i++)
        {
            //知识点
            //1.点加向量 相当于平移点
            //2.四元数 * 向量 相当于在 旋转向量
            line.SetPosition(i, centerPos + Quaternion.AngleAxis(angle * i, Vector3.up) * Vector3.forward * r);
        }
    }
```
### 常按画线
```js
    private LineRenderer line2;
    private Vector3 nowPos;
    void Update()
    {
        //这样写可以画多条线而不是一直连续 ，鼠标每抬起一次就会创建一条新的linerenderer
        if (Input.GetMouseButtonDown(0))
        {
            GameObject obj = new GameObject();
            line2 = obj.AddComponent<LineRenderer>();
            line2.loop = false;
            line2.startWidth = 0.5f;
            line2.endWidth = 0.5f;

            line2.positionCount = 0;
        }
        //使得用户可以长按鼠标画linerenderer线
        if (Input.GetMouseButton(0))
        {
            line2.positionCount += 1;
            //如何得到鼠标转世界坐标的 对应点 
            //知识点

            //1.如何得到鼠标位置
            //Input.mousePosition
            //2.怎么把鼠标 屏幕坐标转世界坐标
            //Camera.main.ScreenToWorldPoint(Input.mousePosition);

            nowPos = Input.mousePosition;
            //需要把位置往前方推一点 才能让摄像机看到
            nowPos.z = 10; //修改z非常关键不然相机会记录不到
            line2.SetPosition(line2.positionCount - 1, Camera.main.ScreenToWorldPoint(nowPos)); //Camera.main.ScreenToWorldPoint非常关键——屏幕坐标转世界坐标操作
        }
    }
```