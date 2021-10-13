# 远景Imposter调研

#### 原理及应用场景      

       Imposter是一种表达复杂3D 模型的简化方法，最经典的例子是用公告板去表示一颗树，通过拍摄烘培模型的渲染结果，然后映射到公告板上，从而达到使用简单公告板表示复杂几何模型的目的。

         Imposter的渲染层级一般在模型最低Lod以下的一个层级，用来表示极远处模型的简化表示，在这个层级的实例模型一般因为限制加载距离的原因而被卸载或者在流式下载的情况尚未加载到。使用一个公告板表示整个模型，既能极大地降低渲染消耗，也不需要加大加载距离以便于看到远处的物件，从而降低游戏的加载内存。

        例如在天涯明月刀ol游戏当中，存在大量使用Imposter的场合，下图中红色是在线烘培的Imposter,绿色是离线烘培的Imposter。[http://km.oa.com/group/29321/articles/show/371290?kmref=search\&from_page=1\&no=3](http://km.oa.com/group/29321/articles/show/371290?kmref=search\&from_page=1\&no=3)

       值得特别注意的，模型的法线和材质等信息也可以烘培起来，以让Imposter能够支持完全的光照，还有巧妙地利用雾效，可以在一定程度上掩盖Imposter带来的视觉瑕疵问题。[\
![1564024516\_71\_w564\_h322.png](http://km.oa.com/files/photos/captures/201907/1564024516\_71\_w564\_h322.png)](http://km.oa.com/group/29321/articles/show/371290?kmref=search\&from_page=1\&no=3)\


#### 具体方法：

* 单公告板(billboard)

![1564044557\_99\_w476\_h293.png](http://km.oa.com/files/photos/captures/201907/1564044557\_99\_w476\_h293.png)

       如果模型位于很远的地方，且模型具有对称的结构，那就是说，不同的朝向，模型的渲染结果相似，那么我们可以用一个单方向的公告板来表示，这种情况建议使用离线烘培一个低分辨率的拍照结果，通过这种方式，仅需绘制一个公告板，就实现对原始模型的替代，因为视距比较远且对视角变化不敏感，远景效果在视觉上尚能接受。

![1564044675\_6\_w473\_h340.png](http://km.oa.com/files/photos/captures/201907/1564044675\_6\_w473\_h340.png)

* 多视角公告板(Imposter)

如果模型离相机距离较近，或者模型并不具有对称结构，例如上图的房子，如果旋转视角，单单使用单一的公告板会使得模型失去三维性，看起来像一个面片的效果，那么这个时候，我们要通过多个视角对原始模型进行拍照，对于这种情况，有通过离线烘培成texture altas来表现模型各个是视角的变化，而对于在线烘培的方式，只保留当前视角的拍照结果，当相机视角偏移达到一定程度再重新生成Imposter, 这种方式需要相机视角发生变化的时候，去切换不同视角下的公告板。![1564024100\_71\_w760\_h770.png](http://km.oa.com/files/photos/captures/201907/1564024100\_71\_w760\_h770.png)

无论是离线烘培还是在线烘培的问题，都容易产生明显的视觉瑕疵，一般采用Dithered transition(old fade out，new fade in,应该是用alpha test的方式）的方法实现物件效果的平滑过渡。

* Box Imposter [ ](https://tomforsyth1000.github.io/papers/gem_imp_filt.html)[https://tomforsyth1000.github.io/papers/gem_imp_filt.html](https://tomforsyth1000.github.io/papers/gem_imp_filt.html)

        由于基于Quad的Imposter的深度只有一层value,当靠近其他的物体的时候，公告板可能会被深度剔除，从而导致显示不完整。

![1564140769\_66\_w918\_h579.png](http://km.oa.com/files/photos/captures/201907/1564140769\_66\_w918\_h579.png)

因此，我们使用包围盒Box来近似表示，通过把6面捕获结果映射到包围盒上，并且只需要绘制包围盒的正面，通过较低频率地更新Box Imposter 贴图，能得到较好的近似原始模型绘制的结果。

![1564142133\_36\_w416\_h388.png](http://km.oa.com/files/photos/captures/201907/1564142133\_36\_w416\_h388.png)

* 9 Card Imposter

对于上面几种方法，当视角发生较大变化的时候，都需要在线重新生成新的Imposter texture,效率受到一定的影响，Ryan Brucks 发明了一种9 Card的Imposter可以直接使用离线的Imposter texture产生具有视差的结果，这种情况最少需要八个边视角的捕捉结果和一个顶视图的捕捉结果，同样渲染的时候也可只绘制正面，剔除掉反面。![1564062472\_67\_w886\_h492.png](http://km.oa.com/files/photos/captures/201907/1564062472\_67\_w886\_h492.png)

* Octahedron Imposter(最新的成果)

9 card Imposter在一定程度也存在转换视角时容易出现视觉瑕疵，Ryan Brucks发明了一种Octahedron Imposter用来解决这个问题，[https://www.youtube.com/watch?v=1xiwJukvb60](https://www.youtube.com/watch?v=1xiwJukvb60)

![1564046116\_80\_w454\_h227.png](http://km.oa.com/files/photos/captures/201907/1564046116\_80\_w454\_h227.png)

或许这个名字应该叫Octahedron Sphere，就是八面体不断细分去表示一个球，然后形成一个捕捉模型视角的捕捉面，由于很多远景对象，相机视点不会位于水平线以下，所以可以使用Hemi-Octahedron捕捉面来捕捉各个视角的结果，从而降低需要烘培的子贴图数量。通过对模型进行捕捉，可以生成一个关于模型各个相机面向的纹理集。

![1564038779\_21\_w754\_h332.png](http://km.oa.com/files/photos/captures/201907/1564038779\_21\_w754\_h332.png)

在游戏运行时，根据当前相机朝向，射线求交找到捕捉面上相交的位置，我们可以得到相对于三个顶点的权重，而每一个顶点即对应一个捕捉位的捕捉结果。我们通过三个捕捉结果对应三个权值的混合，我们得到相应相机视角的捕捉结果。

![1564039202\_33\_w733\_h244.png](http://km.oa.com/files/photos/captures/201907/1564039202\_33\_w733\_h244.png)

这种Imposter能较好的处理不同视角切换时产生的渲染结果过渡不自然的问题，从而可以在中距离内也能替换原始的模型，降低绘制压力。使用这种Imposter不仅降低需要绘制的三角形数量，而且也能表现出与模型最低lod相比更好的效果。

![1564025181\_93\_w669\_h414.png](http://km.oa.com/files/photos/captures/201907/1564025181\_93\_w669\_h414.png)

Unreal 插件：

[https://www.shaderbits.com/blog/octahedral-impostors](https://www.shaderbits.com/blog/octahedral-impostors)

Unity 插件(Amplify Imposters)

[https://80.lv/articles/new-optimization-solution-amplify-impostors/](https://80.lv/articles/new-optimization-solution-amplify-impostors/)

![1564063620\_28\_w882\_h451.png](http://km.oa.com/files/photos/captures/201907/1564063620\_28\_w882\_h451.png)

* 其他的Imposter

1. True Imposters by Nvidia([https://developer.nvidia.com/gpugems/GPUGems3/gpugems3\_ch21.html)](https://developer.nvidia.com/gpugems/GPUGems3/gpugems3\_ch21.htm)

* 支持自阴影，
* 反射，折射，
* 支持查找体积间距离

2.漫威蜘蛛侠里建筑物的Imposter.[http://km.oa.com/group/trdc/articles/show/387912](http://km.oa.com/group/trdc/articles/show/387912)

* 不是真正的Imposter,只是轻量级模型
* 共享一个4k的texture altas
* 不需要face uv,因为是cubemap texture
* 不需要face normal,因为假设轴对齐的

![1564110295\_26\_w506\_h256.png](http://km.oa.com/files/photos/captures/201907/1564110295\_26\_w506\_h256.png)

3.Serious sams4的大地形植被里的Forest Imposter.[http://km.oa.com/articles/show/411192?kmref=search\&from_page=1\&no=9](http://km.oa.com/articles/show/411192?kmref=search\&from_page=1\&no=9)

* 使用八方向的capture textures
* 朝向相机的公告板
* 在Pixel shader里矫正视角，“Interior Mapping" by Joost van Dongen.
* 相机射线求交，混合两个相邻方向的capture texture得到最终结果。
* Albedo, subsrface, normal.ao.depth(self shadow)

![1564110254\_31\_w424\_h371.png](http://km.oa.com/files/photos/captures/201907/1564110254\_31\_w424\_h371.png)
