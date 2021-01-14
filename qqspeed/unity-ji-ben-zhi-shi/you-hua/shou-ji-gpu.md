# 手机GPU

TBR渲染管线与IMR渲染的区别在于，TBR是将屏幕划分为一个个的小块，然后在每个chip分别shading，从而降低带宽消耗，不要与内存发生频繁的交换数据。

Tiled based gpu 不建议开启alpha test 或者discard类似的操作，一旦使用打开alpha test或者其他discard功能的指令，就意味着这个fragment shader上不再只绘制一次像素了。这样会增加额外的性能消耗，所以一般都是建议用不实用alpha test，或者用alpha blend来代替。

Tile-based gpu又分为TBDR（苹果PowerVR）和TBR（高通Adreno，Mali），两者都是在tile里进行渲染，而区别是TBDR有自己的预处理，可以只着色可见像素。

TBDR，以苹果设备用的ImgTec的PowerVR系列来说，在渲染处理时，会在fragment shading 阶段提供，在每个tile使用Deferred的方法，进行**Hidden Surface Removal（HSR）**的处理，原理是fs阶段前，对多边形进行预处理，决定它的哪个像素会对最终结果产生贡献，后面就只对这些像素进行着色。这个功能需要对不透明几何体进行排序。也就是说，要进行这种优化，必须要确保一定有能遮挡的像素，然而使用带有discard的shader指令，例如alpha-test，sample mask，alpha-to-coverage等等，会使得一些本来被遮挡的像素对最终结果产生贡献，所以，这个特性可能只能对一部分物体产生作用，从而产生额外的状态切换消费。以及该fragment额外隐藏像素的处理。

ImgeTec还有另一个depth-only pass功能，生成深度缓冲，再次进行渲染时，就可以获取每个像素的可见深度，只有可见像素才会进行处理。所以，对于苹果设备来说，在CPU阶段对不透明物体的那种从前向后预处理排序是没有必要的。而是应该根据渲染状态来排序。

![ ImgeTec&#x7684;&#x6E32;&#x67D3;&#x5904;&#x7406;](../../../.gitbook/assets/image%20%28100%29.png)

![](../../../.gitbook/assets/image%20%2896%29.png)



{% embed url="https://www.cnblogs.com/TracePlus/p/4037165.html" %}

{% embed url="https://developer.arm.com/solutions/graphics-and-gaming/gaming-engine/unity/arm-guide-for-unity-developers" %}

CPU天梯：[https://www.mydrivers.com/zhuanti/tianti/01/](https://www.mydrivers.com/zhuanti/tianti/01/)

GPU天梯：[https://www.notebookcheck.net/Smartphone-Graphics-Cards-Benchmark-List.149363.0.html](https://www.notebookcheck.net/Smartphone-Graphics-Cards-Benchmark-List.149363.0.html)

![OnChip \(TBR\)](../../../.gitbook/assets/image%20%28103%29.png)

