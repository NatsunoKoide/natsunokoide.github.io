### 射线检测

> 重点内容是 ： 物体信息类 RaycastHit 以及RaycastHit[]数组

> 数组有特殊的api :RaycastAll 用法与 Raycast类似

> 特殊api：RaycastNonAlloc 返回的是碰撞器的数量

```js
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson23 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 什么是射线检测
        //物理系统中 
        //目前我们学习的物体相交判断
        //1.碰撞检测——必备条件 1刚体2碰撞器
        //2.范围检测——必备条件 碰撞器

        //如果想要做这样的碰撞检测呢？
        //1.鼠标选择场景上一物体
        //2.FPS射击游戏（无弹道-不产生实际的子弹对象进行移动）
        //等等 需要判断一条线和物体的碰撞情况

        //射线检测 就是来解决这些问题的
        //它可以在指定点发射一个指定方向的射线
        //判断该射线与哪些碰撞器相交，得到对应对象
        #endregion

        #region 知识点二 射线对象
        //1.3D世界中的射线
        //假设有一条
        //起点为坐标(1,0,0)
        //方向为世界坐标Z轴正方向的射线
        //注意：
        //理解参数含义
        //参数一：起点
        //参数二：方向（一定记住 不是两点决定射线方向，第二个参数 直接就代表方向向量）

        //目前只是申明了一个射线对象 对于我们来说 没有任何的用处
        Ray r = new Ray(Vector3.right, Vector3.forward);

        //Ray中的参数
        print(r.origin);//起点,参数1
        print(r.direction);//方向，参数2

        //2.摄像机发射出的射线
        // 得到一条从屏幕位置作为起点
        // 摄像机视口方向为 方向的射线
        //很重要的API
        Ray r2 = Camera.main.ScreenPointToRay(Input.mousePosition);


        //注意：
        //单独的射线对于我们来说没有实际的意义
        //我们需要用它结合物理系统进行射线碰撞判断
        #endregion

        #region 知识点三 碰撞检测函数
        //Physics类中提供了很多进行射线检测的静态函数
        //注意：
        //射线检测是瞬时的
        //执行代码时进行一次射线检测

        //1.最原始的射线检测
        // 准备一条射线
        Ray r3 = new Ray(Vector3.zero, Vector3.forward);
        // 进行射线检测 如果碰撞到对象 返回true
        //参数一：射线
        //参数二: 检测的最大距离 超出这个距离不检测
        //参数三：检测指定层级（不填检测所有层）
        //参数四：是否忽略触发器 UseGlobal-使用全局设置 Collide-检测触发器 Ignore-忽略触发器 不填使用UseGlobal
        //返回值：bool 当碰撞到对象时 返回 true 没有 返回false

        if (Physics.Raycast(r3, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal))
        {
            print("碰撞到了对象");
        }

        //还有一种重载 不用传入 射线 直接传入起点 和 方向 也可以用于判断
        //就是把 第一个参数射线 变成了 射线的 两个点 一个起点 一个方向
        if (Physics.Raycast(Vector3.zero, Vector3.forward, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal))
        {
            print("碰撞到了对象2");
        }

        //2.获取相交的单个物体信息
        //物体信息类 RaycastHit
        RaycastHit hitInfo;
        //参数一：射线
        //参数二：RaycastHit是结构体 是值类型 Unity会通过out 关键在 在函数内部处理后 得到碰撞数据后返回到该参数中
        //参数三：距离
        //参数四：检测指定层级（不填检测所有层）
        //参数五：是否忽略触发器 UseGlobal-使用全局设置 Collide-检测触发器 Ignore-忽略触发器 不填使用UseGlobal
        if (Physics.Raycast(r3, out hitInfo, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal))
        {
            print("碰撞到了物体 得到了信息");

            //碰撞器信息
            print("碰撞到物体的名字" + hitInfo.collider.gameObject.name);
            //碰撞到的点
            print(hitInfo.point);
            //法线信息
            print(hitInfo.normal);

            //得到碰撞到对象的位置
            print(hitInfo.transform.position);

            //得到碰撞到对象 离自己的距离
            print(hitInfo.distance);

            //RaycastHit 该类 对于我们的意义
            //它不仅可以得到我们碰撞到的对象信息
            //还可以得到一些 碰撞的点 距离 法线等等的信息
        }

        //还有一种重载 不用传入 射线 直接传入起点 和 方向 也可以用于判断
        if (Physics.Raycast(Vector3.zero, Vector3.forward, out hitInfo, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal))
        {

        }

        //3.获取相交的多个物体
        //可以得到碰撞到的多个对象
        //如果没有 就是容量为0的数组
        //参数一：射线
        //参数二：距离
        //参数三：检测指定层级（不填检测所有层）
        //参数四：是否忽略触发器 UseGlobal-使用全局设置 Collide-检测触发器 Ignore-忽略触发器 不填使用UseGlobal
        RaycastHit[] hits = Physics.RaycastAll(r3, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal);
        for (int i = 0; i < hits.Length; i++)
        {
            print("碰到的所有物体 名字分别是" + hits[i].collider.gameObject.name);
        }

        //还有一种重载 不用传入 射线 直接传入起点 和 方向 也可以用于判断
        //之前的参数一射线 通过两个点传入
        hits = Physics.RaycastAll(Vector3.zero, Vector3.forward, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal);

        //还有一种函数 返回的碰撞的数量 通过out得到数据
        if (Physics.RaycastNonAlloc(r3, hits, 1000, 1 << LayerMask.NameToLayer("Monster"), QueryTriggerInteraction.UseGlobal) > 0)
        {

        }
        #endregion
        #region 知识点四 使用时注意的问题
        //注意：
        //距离、层级两个参数 都是int类型
        //当我们传入参数时 一定要明确传入的参数代表的是距离还是层级

        //以下 这样写是错误的 因为第二个参数 代表的是距离 不应该是层级 但这个在ide里面是不会报错的
        if (Physics.Raycast(r3, 1000, 1 << LayerMask.NameToLayer("Monster")))
        {
        }
        #endregion
    }
}

```