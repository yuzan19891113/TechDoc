# 植被的交互调研

对于大世界开阔地形而言，植被是不可缺少的一部分，一个高度可交互，且交互合理的植被系统能极大地增强游戏世界的真实感。对于植被的绘制，这里不做过多的阐述，这里主要集中阐述植被的交互方案和最新的一些进展。植被的交互主要包含与风（或者力\)的交互和与角色\(或者碰撞体\)的交互。

#### 植被与风力的交互

        对于植被而言，一般由几百万个简化模型构成，一般都表现为比较轻和能够弯曲，如果每一颗植被都用刚体或者软体建模明显是不合理的，所以只能用非物理的方法去山寨这种效果。

        一般会在顶点shader里面去模拟风力的运动，最基本的想法是偏移顶点的X,Z根据植被的顶点离地面的高度，这样就能表现出植被根部往上，离地面越高能够弯曲地越多，为了体现风力的效果，sine 函数会用来产生类似风力的扰动，使用合适的参数，能表现出比较好的风与植被的交互效果。

          如果直接用平移偏移的方法去偏移植被顶点，往往会造成植被的拉伸，所以**一般会选择植被的根部作为锚点\(pivot\),然后通过以锚点为中心进行旋转，通过旋转来表现植被摇摆的效果**。一般锚点需要存储在顶点属性里面，所以需要在美术建模工具内将锚点信息以保存，比如[UE4 MAX Pivot Painter 2.0](https://docs.unrealengine.com/zh-CN/Engine/Content/Tools/PivotPainter/PivotPainter2/index.html)

       由于植被里面对于大型的树木，树干与树枝的摇摆是不一致的，例如树干在风吹动下摇摆程度较小，树枝和树叶的摇摆程度会较大，并且树枝的摇摆的旋转中心是在树枝根部而不是树干根部，并且树枝的摇摆是在树干的摇摆的基础上再进行摇摆的，双方具有层次结构关系，所以对于这种情况，需要层次结构关系的Pivot数据,

![1567133349\_63\_w668\_h375.png](http://km.oa.com/files/photos/captures/201908/1567133349_63_w668_h375.png)

       除了Pivot的做法，其他第三方的SDK包括使用speedtree,通过直接设置风力大小，阵风等风力参数可以直接得到较好的风力摆动效果。[https://store.speedtree.com/](https://store.speedtree.com/)

        对于其他一些对草交互效果要求比较高的场合，流体模拟会用来模拟风力的效果，通过将流体移动烘培到texture altas上，可以得到一个精确的虽然在游戏中不能改变的风力场的效果。[https://blogs.unity3d.com/2018/06/15/book-of-the-dead-photogrammetry-assets-trees-vfx/](https://blogs.unity3d.com/2018/06/15/book-of-the-dead-photogrammetry-assets-trees-vfx/)。战神游戏里面 会实时的计算流体场用来做植被的交互或者其他对象的风力交互。

#### 植被与碰撞体的交互

**非物理的方案** 

第一种方法：传入碰撞体的位置及大小信息，在顶点着色器里处理顶点的偏移\(world Position offset\)。由于一般草叶纹理是沿着一个轴向扩展的，也可以使用纹理坐标来判断顶点到叶子根顶点的距离来决定顶点的偏移程度这种方法是一种在没有刷锚点的时候，可以选取的一个较为粗糙的方法，由于直接偏移顶点的xy\(ue4 坐标系统下的xy\),容易出现植被拉伸的问题。[https://www.youtube.com/watch?v=uTCpOOOBfCQ](https://www.youtube.com/watch?v=uTCpOOOBfCQ)

![1567136482\_45\_w1205\_h581.png](http://km.oa.com/files/photos/captures/201908/1567136482_45_w1205_h581.png)

第二种方法：

使用Mesh Distance Field（height field\)求出顶点到最近碰撞面的位置, 

![1564544550\_89\_w1395\_h285.png](http://km.oa.com/files/photos/captures/201907/1564544550_89_w1395_h285.png)

**第三种方法：传入碰撞体的位置及大小信息，根据植被预先定义的Pivot来计算旋转, 通过旋转的方法表现碰撞的效果**，这种做法比直接拉伸顶点的效果要好, 不会出现草被拉长的情况.

![1564544250\_44\_w1768\_h706.png](http://km.oa.com/files/photos/captures/201907/1564544250_44_w1768_h706.png)

这种情况草的Pivot信息是写到了顶点属性里面，写进了顶点的uv2，uv3通道。

![1564544354\_98\_w1077\_h292.png](http://km.oa.com/files/photos/captures/201907/1564544354_98_w1077_h292.png)         ****一般如果采用顶点常量buffer去传入顶点数据，会让shader的复杂度变高，且会限制支持碰撞体的数量，所以一般会采用vertex texture fetch的方法，就是把**大量碰撞体的移动方向或者高度以顶视图的方向渲染到render target**上，然后植被的顶点通过采样贴图，计算出合适的偏移量，由于每一个碰撞体都用一个特效来表达，由于特效具有的时长以及可以慢慢消隐的属性，故还可以表现出当角色经过了某一个植被区域，草可以表现出持续摇摆一段时间然后再静止的效果。

![1567135634\_68\_w587\_h341.png](http://km.oa.com/files/photos/captures/201908/1567135634_68_w587_h341.png)

UE教程：[https://www.youtube.com/watch?v=1bD\_6YFkyZo](https://www.youtube.com/watch?v=1bD_6YFkyZo)

UE实例:[https://www.unrealengine.com/marketplace/zh-CN/slug/advanced-interactive-foliage-system](https://www.unrealengine.com/marketplace/zh-CN/slug/advanced-interactive-foliage-system)

**物理的方案**

使用碰撞体去建模单个植被，为了保证效率，只有角色进入到单个植被的trigger Box内部，静态的植被才会被替换为动态的，被动态骨骼驱动的植被模型。[https://www.ghostofatale.com/dynamic-vegetation-system/](https://www.ghostofatale.com/dynamic-vegetation-system/)

![1564542172\_93\_w1145\_h318.png](http://km.oa.com/files/photos/captures/201907/1564542172_93_w1145_h318.png)

UE实例:[https://www.unrealengine.com/marketplace/zh-CN/slug/uipf-unified-interactive-physical-foliage](https://www.unrealengine.com/marketplace/zh-CN/slug/uipf-unified-interactive-physical-foliage)

UE教程[https://www.youtube.com/channel/UCRAf7dZULce8SRLa9TdgjNw/search?query=Physics+interactable+foliage](https://www.youtube.com/channel/UCRAf7dZULce8SRLa9TdgjNw/search?query=Physics+interactable+foliage)

参考文献

* GPU Gems 3:GPU-Generated Procedrual Wind Animations for Trees

[https://developer.nvidia.com/gpugems/GPUGems3/gpugems3\_ch06.html](https://developer.nvidia.com/gpugems/GPUGems3/gpugems3_ch06.html)

* Vegetation Procedural Animation and Shading in Crysis

[http://km.oa.com/group/39677/articles/show/391183](http://km.oa.com/group/39677/articles/show/391183)

* uchart 4 vegetation:

[http://km.oa.com/group/24148/articles/show/393262](http://km.oa.com/group/24148/articles/show/393262)

* GDC:Between Tech and Art: The Vegetation of 'Horizon Zero Dawn'

 [https://www.gdcvault.com/play/1025530/Between-Tech-and-Art-The](https://www.gdcvault.com/play/1025530/Between-Tech-and-Art-The)

* GDC 2019:Interactive Wind and Vegetation in 'God of War'

[http://km.oa.com/group/39677/articles/show/390704](http://km.oa.com/group/39677/articles/show/390704)

* xoyojank:草的交互的几种实现

[https://blog.csdn.net/xoyojank/article/details/8089388](https://blog.csdn.net/xoyojank/article/details/80893881)

