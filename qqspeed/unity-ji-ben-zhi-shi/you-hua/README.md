# 性能分析

**1. CPU**   
CPU经常受限于需要渲染的**批次\(batches\)数量**。因为CPU每次需要为渲染准备以及收集数据，然后调用GPU图形处理接口，这个过程是相当耗时费力的。   
因此Unity 提供了一个非常好用的工具——**Statistics Window面板**。   
可以通过 **Statistics面板** 检查正在渲染的 **batches**数量，如果数量过高，那么意味着CPU的消耗过高。

* DrawCalls

1. 使用Draw Call Batching，也就是描绘调用批处理。Unity在运行时可以将一些物体进行合并，从而用一个描绘调用来渲染他们。具体下面会介绍。
2. 通过把纹理打包成图集来尽量减少材质的使用。
3. 尽量少的使用反光啦，阴影啦之类的，因为那会使物体多次渲染。

* 物理组件（Physics）
* GC（什么？GC不是处理内存问题的嘛？匹夫你不要骗我啊！不过，匹夫也要提醒一句，GC是用来处理内存的，但是是谁使用GC去处理内存的呢？）

**2. GPU**   
GPU往往受限于在渲染时像素的填充率以及现存带宽的使用。   
如何来检查并定位Unity给出的针对填充率是这样检查的：   
Lower the display resolution and run the game. If a lower display resolution makes the game run faster, you may be limited by fillrate on the GPU.   
降低游戏的分辨率然后运行，如果在低分辨率的情况下，游戏的运行非常流畅，那么可能在GPU上受到的像素填充率的影响。   
Unity同样也提供了一个很好用的工具，来便于我们进行优化—— **FrameDebugger**。

**二、不常见的问题及检查方法：**   
1. **CPU**   
如果在渲染时CPU需要处理太多的**顶点\(vertex\)**，情况如处理**skinned meshes**\(蒙皮网格：用于骨骼动画等\)、**cloth simulation**\(布料仿真\)、**particles system**\(粒子系统\)以及一些游戏里面的**gameObject**和**网格**\(Mesh\)。   
针对于上面的情况，通常在不影响游戏的质量的情况下，尽量保持定点数越低越好，这样才能尽可能的保持CPU的流畅性。针对于此，后面文章会介绍。   
2. **GPU**   
如果在渲染时，GPU也要去处理太多的顶点数据\(Vetex Data\)。   
此时在确保游戏的流畅性的情况下，可以接受的顶点数据的总数，取决于**GPU性能和顶点着色器\(Vertex Shader\)的复杂性**。   
为此Unity给出的Tips：   
针对于**移动设备**，尽量当前每帧渲染**顶点数不要超过十万个**。   
针对于**PC设备**,尽管PC设备的性能优于移动设备，也能更好的处理顶点数据，甚至能同时处理几百万个，但是仍然可以通过优化这个，然后仍然可以获得一个非常好的额实际性能效果。   
3. **Garbage Collection\(GC 垃圾回收\)或者Physics**   
如果渲染时，通过检查定位，发现其实影响性能的并非是因为CPU的Batches或者GPU引起的，那么，这个问题可能出GC或者Physics（物理计算）。

###  <a id="cpu&#x51CF;&#x5C11;draw-call"></a>

