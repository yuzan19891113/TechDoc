# 手机GPU

手机没有专有的显存而是共用内存，带宽瓶颈发生onchip在与内存的数据交换上。

TBR渲染管线与IMR渲染的区别在于，TBR是将屏幕划分为一个个的小块，然后在每个chip分别shading，从而降低带宽消耗，不要与内存发生频繁的交换数据。

CPU天梯：[https://www.mydrivers.com/zhuanti/tianti/01/](https://www.mydrivers.com/zhuanti/tianti/01/)

GPU天梯：[https://www.notebookcheck.net/Smartphone-Graphics-Cards-Benchmark-List.149363.0.html](https://www.notebookcheck.net/Smartphone-Graphics-Cards-Benchmark-List.149363.0.html)



### GPU架构

#### 基本介绍

ALU:多个算术逻辑单元，多个ALU共享一个指令流。 SIMD，每个像素一个Ctx\(contextstoriage\)，像素中的ctx存储临时变量，shared ctx data储存 const buffer等。

![ShaderCore&#x4E00;&#x6B21;&#x5904;&#x7406;8&#x4E2A;&#x50CF;&#x7D20;](../../../.gitbook/assets/image%20%28126%29.png)

shader可能的性能瓶颈：

stall: 纹理采样，读取顶点数据，读取varing\(指令有外部依赖\)

cacheMiss（更小的texture sample，更好的cache\)



总共有2种基于TBR的GPU结构

#### TBR\(Mali,Andreno, android\)

1. Chip读取主内存顶点数据执行vertex shader，变换到齐次裁剪空间，顶点组装成三角形流，Tilerling 划分，写入主内存。
2. Chip 读取主内存当前Tiler内的三角形流shading ,alpha test, alpha blend 
3.  on chip buffer 写回主内存。

![TBR](../../../.gitbook/assets/image%20%28124%29.png)

#### TBDR\(power VR, ios\)

![TBDR](../../../.gitbook/assets/image%20%28123%29.png)

**与TBR的区别在于在栅格化后有硬件级别的HSR,来剔除不必要的像素**。**并且Alphatest以及discard在TBDR上是优化。**

TBDR，以苹果设备用的ImgTec的PowerVR系列来说，在渲染处理时，会在fragment shading 阶段提供，在每个tile使用Deferred的方法，进行**Hidden Surface Removal（HSR）**的处理，原理是fs阶段前，对多边形进行预处理，决定它的哪个像素会对最终结果产生贡献，后面就只对这些像素进行着色。这个功能需要对不透明几何体进行排序。也就是说，要进行这种优化，必须要确保一定有能遮挡的像素，然而使用带有discard的shader指令，例如alpha-test，sample mask，alpha-to-coverage等等，会使得一些本来被遮挡的像素对最终结果产生贡献，所以，这个特性可能只能对一部分物体产生作用，从而产生额外的状态切换消费。以及该fragment额外隐藏像素的处理。

ImgeTec还有另一个depth-only pass功能，生成深度缓冲，再次进行渲染时，就可以获取每个像素的可见深度，只有可见像素才会进行处理。所以，对于苹果设备来说，在CPU阶段对不透明物体的那种从前向后预处理排序是没有必要的。而是应该根据渲染状态来排序。



{% embed url="https://www.cnblogs.com/TracePlus/p/4037165.html" %}

### GPU性能指标

GFLOPS:绘制一个全屏矩形的帧率 1024 \* 768 \* 1000 \* 2 \* 30 = 47.2 GFLOPS

纹理采样 Gtex/sec

纹理填充 Gpix/sec

### 发热

#### gpu发热： 影响性能

gpu时间

#### 带宽发热：影响耗电时间，玩得久

顶点数据：normal tangent 用 1010102， uv 用half float

贴图带宽：ASTC, ETC, MipMap



### 分析工具

snapdragon profiler

XCode instrument

### shader性能分析

 ALU, Texture, 带宽， Stall, ROP

Forward PBR渲染通常

高配机：ALU Bound

低配机：Stall Bound

PS消耗高可能是全屏面积大，计算可以从ps移动到vs计算，但当物体显示很小时，未必划算。

Branch性能：

一个group中，所有像素结果相同，则只会走一边。如果有的走一个分支，有的走其他分支，则效率低。

临近像素能够走相同的分支。

![](../../../.gitbook/assets/image%20%28127%29.png)

### Shader 编译

HLSL2GLSL -&gt; GLSL Optimizer -&gt;Driver

编译器自动优化的工作：

1. 去掉没有引用的代码和变量
2. 代码中常量预计算
3. 自动劣化
4. 要看生成代码













