# 实时阴影

**研发前：**

unity设置关闭lightmap static选项

参考刺激战场：中低配都没有实时阴影，只有高清以上才有

* shadow mapping \(中高档） 优先级:高

1. 只保存一个rt，切分为4分，通过projection矩阵将切分区域的对象的投到rt不同位置
2. Light Space Perspective ShadowMap
3.  PCF + ESM过滤得到软阴影
4. 可能需要处理动态阴影和bake阴影的问题
5.  优化：分辨率，隔帧更新，shader,blur优化

![1 rt,4&#x4E2A;viewport](../../.gitbook/assets/image%20%2857%29.png)

![](../../.gitbook/assets/image%20%2852%29.png)

[http://km.oa.com/group/1667/articles/show/265715?kmref=author\_post](http://km.oa.com/group/1667/articles/show/265715?kmref=author_post)

[http://km.oa.com/group/1667/articles/show/265718?kmref=author\_post](http://km.oa.com/group/1667/articles/show/265718?kmref=author_post)

* bake + rotate projector\(中低档\)优先级:中

1. bake 一个标准角度的阴影projector结果
2. 旋转 projector矩阵 as object rotate at fixed rotation

**研发中：**

* 动态实时阴影，静态bake 阴影

![](../../.gitbook/assets/image%20%2868%29.png)

* 动态物件投射静态

![](../../.gitbook/assets/image%20%2869%29.png)

