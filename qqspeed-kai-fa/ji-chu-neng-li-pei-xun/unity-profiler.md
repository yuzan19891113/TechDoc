# Unity profiler



## Unity Profiler入门 <a href="unity-profiler-e5-85-a5-e9-97-a8" id="unity-profiler-e5-85-a5-e9-97-a8"></a>

### 目录 <a href="e7-9b-ae-e5-bd-95" id="e7-9b-ae-e5-bd-95"></a>

* 前言
* Profiler简介
* 案例1：在编辑器中检视峰值
* 案例2：安卓真机上使用Deep Profile模式分析
* 各视窗(模块)介绍
* 附加内容

### 前言 <a href="e5-89-8d-e8-a8-80" id="e5-89-8d-e8-a8-80"></a>

本篇wiki是面向新手的**入门级**教程，意在快速让新人上手**Unity Profiler**的使用，并进行简单的性能分析。\
因此我们将两个简单的实用案例放在前面，希望可以通过它们**先熟悉使用流程**，而关于工具本身各模块的介绍则放在之后

### Profiler简介 <a href="profiler-e7-ae-80-e4-bb-8b" id="profiler-e7-ae-80-e4-bb-8b"></a>

**Unity Profiler**是Unity中分析性能开销的工具\
_**- 各种开销一览无遗**_\
_**- 可跨平台使用（Web、PC、iOS、Android、WP）**_\
\
要查看性能分析数据，需要在编辑器中使用性能分析功能运行游戏并记录性能数据，然后在分析器窗口时间轴上显示数据。通过点击时间线中的任何位置，Profiler窗口的底部将显示所选帧的详细信息。

![Profiler窗口](https://iwiki.woa.com/download/attachments/909974198/image-1627653533788.png?version=1\&modificationDate=1627653533796\&api=v2)

### 案例1：在编辑器中检视峰值 <a href="e6-a1-88-e4-be-8b1-ef-bc-9a-e5-9c-a8-e7-bc-96-e8-be-91-e5-99-a8-e4-b8-ad-e6-a3-80-e8-a7-86-e5-b3-b0" id="e6-a1-88-e4-be-8b1-ef-bc-9a-e5-9c-a8-e7-bc-96-e8-be-91-e5-99-a8-e4-b8-ad-e6-a3-80-e8-a7-86-e5-b3-b0"></a>

#### 1.准备工作 <a href="1-e5-87-86-e5-a4-87-e5-b7-a5-e4-bd-9c" id="1-e5-87-86-e5-a4-87-e5-b7-a5-e4-bd-9c"></a>

* 打开Profiler窗口，通过工具栏** Window > Analysis> Profiler **访问Unity编辑器中的Profiler窗口\

* 将连接目标选为`Playmode`，并激活相邻右侧的`录制`按钮(红色为激活状态)。这样在编辑器进入playmode时，Profiler便会自动立即开始录制\

* (可选)在模块菜单中勾选一些模块，例如勾选`GPU`以在录制中显示GPU使用情况。_tips：模块信息仅会在勾选启用后才开始捕获，也就说如果在录制中途才启用某模块，则在它启用前的所有用量都没有记录_\


![附加模块](https://iwiki.woa.com/download/attachments/909974198/image-1627720187202.png?version=1\&modificationDate=1627720187227\&api=v2)

![准备工作](https://iwiki.woa.com/download/attachments/909974198/image-1627718519657.png?version=1\&modificationDate=1627718519695\&api=v2)

![Window > Analysis> Profiler](https://iwiki.woa.com/download/attachments/909974198/image-1627653515255.png?version=1\&modificationDate=1627653515258\&api=v2)

#### 2.捕获一段Profile信息 <a href="2-e6-8d-95-e8-8e-b7-e4-b8-80-e6-ae-b5profile-e4-bf-a1-e6-81-af" id="2-e6-8d-95-e8-8e-b7-e4-b8-80-e6-ae-b5profile-e4-bf-a1-e6-81-af"></a>

* 点击`播放`按钮，在编辑器中运行游戏
* 录制一段时长后，点击`暂停`按钮；或者在时间轴中点击任意一帧，这两个方式均能同时暂停游戏和录制
* 如果要停止录制，可以再次点击`录制`按钮，通过这种方式仅会终止录制而不会暂停游戏

#### 3.检视峰值 <a href="3-e6-a3-80-e8-a7-86-e5-b3-b0-e5-80-bc" id="3-e6-a3-80-e8-a7-86-e5-b3-b0-e5-80-bc"></a>

* 点击`CPU Usage`时间轴上的一处峰值，可以看到Profiler下方列出了该帧的CPU使用情况\

* 点击上图中①处下拉菜单，可以切换不同的视图\

* 点击不同模块的时间轴，可以浏览该模块对应帧的信息\


![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628926738246.png?version=1\&modificationDate=1628926738278\&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628926852000.png?version=1\&modificationDate=1628926852031\&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628926665645.png?version=1\&modificationDate=1628926665672\&api=v2)

### 案例2：安卓手机上使用ADB连接Profiler <a href="e6-a1-88-e4-be-8b2-ef-bc-9a-e5-ae-89-e5-8d-93-e6-89-8b-e6-9c-ba-e4-b8-8a-e4-bd-bf-e7-94-a8adb-e8-bf" id="e6-a1-88-e4-be-8b2-ef-bc-9a-e5-ae-89-e5-8d-93-e6-89-8b-e6-9c-ba-e4-b8-8a-e4-bd-bf-e7-94-a8adb-e8-bf"></a>

#### 1.准备工作 <a href="1-e5-87-86-e5-a4-87-e5-b7-a5-e4-bd-9c-3" id="1-e5-87-86-e5-a4-87-e5-b7-a5-e4-bd-9c-3"></a>

* 安装[Android Debug Bridge (adb)](https://developer.android.com/studio/command-line/adb)，并确保可以使用`adb`指令
* 手机开启开发者模式和USB调试
* 连接手机至PC，确保设备出现在adb设备列表中（`adb devices`）\


![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628924885759.png?version=1\&modificationDate=1628924885787\&api=v2)

#### 2.启动Profiler并连接 <a href="2-e5-90-af-e5-8a-a8profiler-e5-b9-b6-e8-bf-9e-e6-8e-a5" id="2-e5-90-af-e5-8a-a8profiler-e5-b9-b6-e8-bf-9e-e6-8e-a5"></a>

* 运行游戏（使用`Debug`包）
* 在 Profiler 连接目标下拉菜单中选择 `AndroidPlayer(ADB@...)`，开始录制**（如果没有出现该选项，请继续以下步骤）**
* 如果没有出现连接目标，我们需要使用ip+端口连接到设备：先使用adb映射端口`adb forward tcp:54999 localabstract:Unity-com.tencent.tmgp.speedmobile`，其中`com.tencent.tmgp.speedmobile`是bundle identifier，可以在`File -> Build Settings -> Player Settings -> Player`中查询
* 如果映射失败，可以尝试换用`[54998, 55511]`范围内任意端口，映射成功会返回指定端口\

* 在 Profiler 连接目标下拉菜单选择`<Enter IP>`，填写`本机IP:adb映射端口`，例如 `127.0.0.1:54999`\

* 点击`Connect`，开始录制\


![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628925758173.png?version=1\&modificationDate=1628925758203\&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628925688935.png?version=1\&modificationDate=1628925688964\&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628925946373.png?version=1\&modificationDate=1628925946406\&api=v2)

### 各视窗(模块)介绍 <a href="e5-90-84-e8-a7-86-e7-aa-97-e6-a8-a1-e5-9d-97-e4-bb-8b-e7-bb-8d" id="e5-90-84-e8-a7-86-e7-aa-97-e6-a8-a1-e5-9d-97-e4-bb-8b-e7-bb-8d"></a>

#### 工具栏 <a href="e5-b7-a5-e5-85-b7-e6-a0-8f" id="e5-b7-a5-e5-85-b7-e6-a0-8f"></a>

![工具栏](https://iwiki.woa.com/download/attachments/909974198/image-1627654452049.png?version=1\&modificationDate=1627654452068\&api=v2)

* 1.模块菜单\
  可以通过此处的下拉菜单过滤显示的模块\
  ![模块菜单](https://iwiki.woa.com/download/attachments/909974198/image-1627654700012.png?version=1\&modificationDate=1627654700029\&api=v2)
* 2.连接目标\
  通过此处的下拉菜单选择连接并分析的实例，\
  ![连接目标](https://iwiki.woa.com/download/attachments/909974198/image-1627654839364.png?version=1\&modificationDate=1627654839385\&api=v2)
* 3.录制\
  记录或暂停
* 4.显示帧指针
* 5.清空捕获数据
* 6.ClearOnPlay
* 7.DeepProfile\
  普通的分析只记录常见的Unity回调（Awake、Start、Update和FixUpdate）所返回的时间和内存分配信息\
  启用 Deep 选项可以用更深层次的指令重新编译脚本，允许它统计每个调用方法
* 8.CallStack
* 9.文件操作\
  允许用户保存/加载Profile文件
* 10.细分视图模式\
  **Timeine**：更直观的显示每次调用的耗时占比，垂直组织到不同的部分，代表运行时的不同线程\
  \
  **Hierarchy**：更直观的显示调用关系，可在右侧看到选中函数的调用堆栈，合并类似的数据元素和Unity的全局函数调用\
  BeginGUI() 和 EndGUI() 会合并到一起\
  **用途：看一下哪个函数调用会花费最多的时间**\
  \
  **Raw Hierarchy** 模式\
  不会合并\
  **用途：统计某个全局方法调用了多少次**
* 11.Live模式\
  仅能在编辑器中使用（PlayMode或者Editor），开启后能实时显示当前帧的分析，但是会增加开销

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628927818305.png?version=1\&modificationDate=1628927818333\&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/909974198/image-1628927853558.png?version=1\&modificationDate=1628927853590\&api=v2)

#### CPU Usage模块 <a href="cpu-usage-e6-a8-a1-e5-9d-97" id="cpu-usage-e6-a8-a1-e5-9d-97"></a>

1. 主要函数调用的CPU时间开销 :
   * 最耗时的操作会显示在Hierarchy面板的最上方
2. Mono分配内存的情况 :
   * 在GC Alloc栏

#### GPU Usage模块 <a href="gpu-usage-e6-a8-a1-e5-9d-97" id="gpu-usage-e6-a8-a1-e5-9d-97"></a>

1. 统计Drawcall的数量及耗费时间
2. Profiler中Drawcall是广义上的Drawcall，包含：
   * GPU Stage切换
   * Clear操作
   * 将顶点数据传输到GPU的开销
   * 绘制调用（狭义上的Drawcall）
   * Rendering模块和Game Statistics里统计的是这种Drawcall

#### Memory模块 <a href="memory-e6-a8-a1-e5-9d-97" id="memory-e6-a8-a1-e5-9d-97"></a>

1. 查看内存使用细节
   * 点击 可看各种资源内存占用的情况
   * 点击后，获得当前内存情况。
2. 内存引用关系图
   * 可以观察到一个对象在哪里被引用，利于检测内存泄露

## 附 <a href="e9-99-84" id="e9-99-84"></a>

### 1.各参数简介 <a href="1-e5-90-84-e5-8f-82-e6-95-b0-e7-ae-80-e4-bb-8b" id="1-e5-90-84-e5-8f-82-e6-95-b0-e7-ae-80-e4-bb-8b"></a>

#### CPU Usage <a href="cpu-usage" id="cpu-usage"></a>

* `GC Alloc` - 记录了游戏运行时代码产生的堆内存分配。这会导致ManagedHeap增大，加速GC的到来。我们要尽可能避免不必要的堆内存分配，同时注意：1、检测任何一次性内存分配大于2KB的选项；2、检测每帧都具有20B以上内存分配的选项。
* `WaitForTargetFPS` - VSync功能所致，即显示的是当前帧的CPU等待时间。
* `Overhead` - 表示Profiler总体时间，即所有单项的记录时间总和。用于记录尚不明确的时间消耗，以帮助进一步完善Profiler的统计。（一般出现在移动设备，锯齿状为Vsync所致）
* `Physics.Simulate` - 当前帧物理模拟的CPU占用量。
* `Camera.Render` - 相机渲染准备工作的CPU占用量。
* `RenderTexture.SetActive` - 设置RenderTexture操作。比对当前帧与前一帧的ColorSurface和DepthSurface，如果一致则不生成新的RT，否则生成新的RT，并设置与之对应的Viewport和空间转换矩阵。
* `Monobehaviour.OnMouse` - 用于检测鼠标的输入消息接收和反馈，主要包括 SendMouseEvents和DoSendMouseEvents。
* `HandleUtility.SetViewInfo` - 仅用于Editor中，作用是将GUI在Editor中的显示看起来与发布版本上的显示一致。
* `GUI.Repaint` - GUI的重绘（尽可能避免使用Unity内建GUI）。
* `Event.Internal_MakeMasterEventCurrent` - 负责GUI的消息传送。
* `Cleanup Unused Cached Data` - 清空无用的缓存数据，主要包括RenderBuffer 的垃圾回收和TextRendering的垃圾回收。
* `RenderTexture.GarbageCollectTemporary` - 存在于RenderBuffer的垃圾回收中，清除临时的FreeTexture。
* `TextRendering.Cleanup` - TextMesh的垃圾回收操作。
* `Application.Integrate Assets in Background` - 遍历预加载的线程队列并完成加载，同时完成纹理的加载、Substance的Update等。
* `Application.LoadLevelAsync Integrate` - 加载场景的CPU占用。
* `UnloadScene` - 卸载场景中的GameObjects、Component和GameManager，一般用在切换场景时。
* `CollectGameObjects` - 将场景中的GameObject和Component聚集到一个Array 中。
* `Destroy` - 删除GameObject或Component的CPU占用。
* `AssetBundle.LoadAsync Integrate` - 多线程加载AwakeQueue中的内容，即多线程执行资源的AwakeFormLoad函数。
* `Loading.AwakeFormLoad` - 在资源被加载后调用，对每种资源进行与其对应的处理。
* `StackTraceUtility.PostprocessStacktrace() & StackTraceUtility.ExtractStackTrace()` - 一般是由Debug.Log或类似API造成，游戏发布后需将Debug API进行屏蔽。
* `GC.Collect` - 系统启动的垃圾回收操作。当代码分配内存过量或一定时间间隔后触发，与现有的Garbage size及剩余内存使用粒度相关。
* `GarbageCollectAssetsProfile` - 引擎在执行UnloadUnusedAssets操作。
* `Shader.Parse` - 资源加入后引擎对Shader的解析过程。

#### Memory <a href="memory" id="memory"></a>

* `GameObjects in Scene` - 当前帧场景中的GameObject数量。
* `Total Objects in Scene` - 当前帧场景中的Object数量（除了GameObject外，还有Component等）。
* `Total Object Count` - Object数量 + Asset数量。
* `Scene Memory` - 记录当前帧场景中各方面的内存占用情况，包括GameObject、所有资源、各种组件及GameManager等。
