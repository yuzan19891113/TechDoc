# XCode Instructment

有两种方式进行ios截帧 

1.拿到gpu trace数据 在xcode 上replay 

2.在xcode上运行xcode projects 进行截帧



**环境**

Unity2017.4.17 macOS 10.15.2 iPhone xr OS 13.3 XCode 11.2 苹果开发者账号

测试工程 UnityChan crs

### 总览

由于unitychan crs是Unity5 的工程 ，转到2017之后需要把一些后处理脚本和shader删除掉才能兼容，导出xcode工程，注意Graphics API 要选择 Metal，编译到手机上之后就可以看到gpu相关的信息

编译运行之后xcode显示实时profile信息

![](https://pic4.zhimg.com/80/v2-6f8ef11bf563521b8822112d841871df_hd.jpg)

依次点击左上角的show debug navigator，FPS 就可以看到gpu的整体消耗，这里可以很方便的看到一些重要的初级信息，包括fps，每帧的cpu时间，gpu时间，当前帧的shader消耗排序。

当我们想要获取一些详细的信息的时候，就要通过截取一帧来详细分析了，点下面的相机图标。

**Frame Graph**

首先可以看一下整体的绘制流程和节点间的依赖关系渲染一帧的Frame Graph

![](https://pic1.zhimg.com/80/v2-903649d1ed6448e2ad3b9fcf8e4c5d30_hd.jpg)

包括每个相机的绘制顺序，内容，rt格式，dc数量，gpu耗时，比如场景中为实现大屏幕效果先进行的渲染。特写镜头相机绘制的概览，包括DC数，顶点数，RT格式，尺寸等

![](https://pic4.zhimg.com/80/v2-05ef3f030f47507e36454d4334d25307_hd.jpg)

在左边的Draw call list 中可以选择当前场景中任何一个DC,中间绿色标出的就是当前绘制的内容，注意一下下面红框的内容，标出了vertex shader 和fragment shader 分别的耗时。

### 性能分析

除了最简单总体gpu/cpu耗时，截帧之后的界面会将gpu耗时的统计展示出来，这样就可以快速定位gpu性能热点。每个shader的消耗统计

![](https://pic4.zhimg.com/80/v2-e8749242f7dfe2a012fb0408f69bba8b_hd.jpg)

其次要看的就是整个渲染流程中每个阶段的耗时，这时候直接看左边EncounterEncounter中的Encode模式Encounter中的Draw模式

![](https://pic3.zhimg.com/80/v2-af5041a2190097ff4c7e7656af8d177e_hd.jpg)

![](https://pic3.zhimg.com/80/v2-2407ec105b5ef37cc38d1f4225d66dca_hd.jpg)

两种模式，Encoder模式看每个encoder的消耗，Draw模式看每个DC的消耗，可以发现，渲染小尺寸rt的话，顶点的消耗比较多，正式渲染场景的时候fragment消耗更多。

再细一些，可以看到vertex shader 和fragment shader的分别耗时。

performance 和pipeline statics里面还有很多有用的信息，比如shader指令数，采样数，texture cache miss 率等，对于性能分析还是很有用的。当个DrawCall的Perfoamance统计

![](https://pic4.zhimg.com/80/v2-9c0636b50cb181da71b2a63e048cf15f_hd.jpg)

但是如果是单个shader中需要精确定位消耗，是计算耗还是采样耗，cache不友好耗，还是贷款问题，之前是只能靠经验总结，或者“开关Test”，这样其实是非常不精确的，新版本的xcode解决了这个问题，消耗可以精确到shader中具体的每行代码！

在Bound Resources 中找到对应的vertex shader 或者fragment shader，![](https://pic1.zhimg.com/80/v2-3de6bc1f1355c0ad64b0950de9f3537c_hd.jpg)Shader中的函数消耗统计

函数入口标注的是整个函数耗时，点一下小圆圈可以看到分类百分比。

ALU: 在GPU的数学逻辑运算中消耗的时间，尽可能用half而不是用float，同时减少使用复杂的运算指令，比如sqrt，sin，cos，recip；

Memory:访问buffer或者存储所用的时间，通过降低贴图分辨率或者降采样\(mipmap\)来优化。

控制流：用于条件判断，自增，或者跳转指令造成的shader分支和循环消耗。迭代次数尽量用常数来控制，metal的编译器可以对这样的代码进行优化。

同步：类似于多线程在执行之前对资源的等待

具体到每行shader代码，我们也可以看到他们的消耗![](https://pic2.zhimg.com/80/v2-86d0d03cbb30048f95f38226ae3d994d_hd.jpg)每行代码消耗的百分比

通过这些数据，我们就能快速定位到shader内部的热点了。

贴一下Unity给出的shader性能建议，挺在理![](https://pic2.zhimg.com/80/v2-5d3dc7699837e2c9874d8a62ea118289_hd.jpg)![](https://pic2.zhimg.com/80/v2-0b03c4326ca5ca512b9a465b0ea97225_hd.jpg)

### CPU Bound or GPU Bound? 可能是个伪命题

一些老牌的优化大佬可能会和你说优化的第一步就是定位出到底是GPU Bound 和CPU Bound 然后去针对优化，然而这样的做法在移动平台上不一定是适用的。![](https://pic1.zhimg.com/80/v2-24662e49be85b05f8acc26606c5ae13c_hd.jpg)如何定位CPU Bound 还是GPU Bound

因为移动平台需要考虑的一个重要的影响因素就是发热耗电，当手机严重发热的时候，不仅是对玩家体验造成很大的影响，更会让cpu降频，降频之后不管你cpu优化的多牛x，都给我GG斯密达。

造成发热耗电的很大的一个影响因素就是GPU消耗，按照本人的优化经验，60fps模式，当GPU消耗超过10ms的时候就会引起设备发热，如果游戏一启动，直接“GPU Bound”（60帧下GPU消耗超过16.66ms）了，那基本几分钟内手机就会热到爆炸。

所以在移动设备上去谈性能优化的时候，可能发热耗电才是最需要考虑的。

下面说一下一些shader的调试方法。

### Update Shaders Live

基本是抓帧工具的基础功能，虽然简单但是非常强大，在没有比较强力的debug手段之前，能处理大部分的问题。

截帧之后在bound resource中找到对应的vertex shader 和 fragment shader，打开代码之后修改对应的代码，点击reload 按钮就可以看到修改之后的shader的效果，同时手机上的shader也会更新。![](https://pic1.zhimg.com/80/v2-34daadca405a605d6e080ae6194cd790_hd.jpg)实时调试shader 的效果

### 调试vertex shader

有些时候我们可能会遇到一些因为顶点数据出错的问题，比如点色错误导致渲染发黑，顶点法线错误导致高光计算错误，uv错误导致采样出错等等，这时候我们如果能输出顶点的相关信息就能很快定位问题。![](https://pic4.zhimg.com/80/v2-0d00108ed8c2f4581b5e11f15945967f_hd.jpg)开发过程中遇到的某个模型渲染变黑的问题

选择Draw call中的Geometry，就可以看到当前渲染的网格，鼠标点击其中的某个三角形，下面就会显示出当前三角形的输入信息和输出信息。![](https://pic3.zhimg.com/80/v2-ec7321cb857195a806c6721110595246_hd.jpg)Debug模式下的网格信息

如果还要深入看vertex shader中shader的执行过程以及中间的变量值，点击面板的debug按钮，可以看到展开的shader，右边的数据就是当前三角形在此vertex shader中执行的过程了，左边的列表就是单步调试，下面就是监视器。![](https://pic3.zhimg.com/80/v2-138c65429a51ea482b48fbc76b6f6a0e_hd.jpg)

shader是unity 编译出来的metal的shader 代码，虽然有点难看，但是比汇编简直不要好太多。

### 调试像素信息

像素级别的渲染问题出现的就更多了，变黑，烂面，平台显示效果不一致，贴图丢失等等，如果无法截帧的话，很难定位这些问题。xcode提供了很多强大的调试功能。

在xcode中截帧之后，在bound resources中可以看到所有的贴图，对应的就是材质球上的贴图槽![](https://pic1.zhimg.com/80/v2-0dafc0b4bfac4d1260a519237ce164b4_hd.jpg)Draw Call的贴图信息，精准调试像素

如果想要调试某个像素，点小甲壳虫，然后在弹窗中选择想要debug的像素，点debug按钮。![](https://pic4.zhimg.com/80/v2-2cb6633c91080334d8309e2f399f6ddf_hd.jpg)鼠标悬停在变量之上能够同时看到周围像素的值

和vertex shader debug 模式一样，这个像素上的计算过程的中间值都已经显示出来，中间过程的渲染结果直接把鼠标悬停在变量上面就可以显示出来，而且在弹出的小窗里面可以看到同一个tile里面的像素值，简直不要太强大，这种profiler应该只有自家做硬件才能做出来吧。

### 小结

进化到11.2版本的xcode应该是市面上的最好的图形调试工具了，唯一一个缺点应该是只能截取自己工程，其他方面完全吊打对手。能够快速调试定位渲染问题，分析渲染流程，精确定位性能消耗，赶紧用起来！

还有更多的关于xcode的使用技巧以及metal的知识可以去看下wwdc的相关的课程。

