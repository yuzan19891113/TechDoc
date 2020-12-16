# 实时阴影

* shadow mapping \(中高档） 优先级:高

1. 只保存一个rt，切分为4分，通过projection矩阵将切分区域的对象的投到rt不同位置
2. Light Space Perspective ShadowMap
3.  PCF + ESM过滤得到软阴影
4. 可能需要处理动态阴影和bake阴影的问题
5.  优化：分辨率，隔帧更新，shader,blur优化

* bake + rotate projector\(中低档\)

1. bake 一个标准角度的阴影projector结果
2. 旋转 projector矩阵 as object rotate at fixed rotation

