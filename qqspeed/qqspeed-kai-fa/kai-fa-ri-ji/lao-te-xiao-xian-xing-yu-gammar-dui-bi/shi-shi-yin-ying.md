# 实时阴影

* shadow mapping \(中高档） 

1. 只保存一个rt，切分为4分，通过projection矩阵将切分区域的对象的投到rt不同位置
2. Light Space Perspective ShadowMap
3.  PCF + ESM过滤得到软阴影
4.  优化：分辨率，隔帧更新，shader,blur优化

* bake + rotate projector\(中低档\)

