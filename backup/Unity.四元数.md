### 轴-角对
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/fb7ce126-6726-4819-bebf-a2dfa19a1f75)

### unity中的四元数结构体 —— Quaternion
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/db2d3b39-ca81-4b03-85ec-c2898ba6e7ae)
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/90418e9d-fb10-4fc1-a528-0ea20316fe95)

> 四元数公式api的使用用例
```js
//写一个 绕轴x 60度旋转 的cube
        Quaternion q = new Quaternion(Mathf.Sin(30 * Mathf.Deg2Rad), 0, 0, Mathf.Cos(30) * Mathf.Deg2Rad);
        GameObject obj = GameObject.CreatePrimitive(PrimitiveType.Cube);
        obj.transform.rotation = q; 
//q2与q相同 只是api不同
        Quaternion q2 = Quaternion.AngleAxis(60, Vector3.right);
```

> 四元数和欧拉角转换
```js
        //1.欧拉直接赋值给四元数
        Quaternion q3 = Quaternion.Euler(120, 0, 0);
        obj.transform.rotation = q3;
        //2.四元数转欧拉角
        print(q3.eulerAngles);
```

> 四元数相乘——旋转（不会万向节死锁）
```js
        this.transform.rotation *= Quaternion.AngleAxis(1, Vector3.forward); 
```

> !!!!写一种会产生万向节死锁的方法（错误案例）
```js
        e = this.transform.rotation.eulerAngles;
        e += Vector3.forward;
        this.transform.rotation = Quaternion.Euler(e);
```
### 单位四元数——代表没有旋转
![image](https://github.com/NatsunoKoide/NatsunoKoide.github.io/assets/137853852/7e3c13a5-2def-426e-a612-2443e74416f6)

```js
#region 知识点1 单位四元数
        print(Quaternion.identity);
        //直接让单位四元数赋值会直接让物体转角全部归零
        //obj.rotation = Quaternion.identity;

        //Instantiate(obj, Vector3.zero, Quaternion.identity);
 #endregion
```

### 四元数中的差值运算 + 通过看向的方式让物体一直盯着目标
```js
    public Quaternion start;
    public float time;

    public Transform lookA;
    public Transform lookB;
void Start()
    {
        start = B.transform.rotation;
    }
void Update()
    {

        #region 知识点2 插值运算 (在Quaternion中有lerp和slerp 但是lerp效果在大角度中一般所以就用slerp)
        //无限接近
        A.transform.rotation = Quaternion.Slerp(A.transform.rotation, target.transform.rotation, Time.deltaTime);
        //匀速  time >= 1 到达目标
        time += Time.deltaTime;
        B.transform.rotation = Quaternion.Slerp(start, target.rotation, time);
        #endregion
        #region 知识点3 lookrotation
        //Quaternion q = Quaternion.LookRotation(lookB.position - lookA.position);
        lookA.rotation = Quaternion.LookRotation(lookB.position - lookA.position);
        #endregion
```