# Editor 截帧

## 简单介绍

\*\*帧调试器 \(Frame Debugger\) 可将正在运行的游戏的状态冻结到特定帧来自由回放，并查看用于渲染该帧的各个\_绘制调用\_。除了列出绘制调用，调试器还可逐个单步执行这些调用，以便您详细查看场景是如何从场景的图形元素构建的。

## 工具使用

1. 打开Frame Debugger 窗口，菜单栏中 Window --&gt; Analysis --&gt; \_\_Frame Debugger\_\_。
2. 点击下图红框处，变为"Disbale"，就会显示绘制调用信息。 主列表以层级视图形式显示绘制调用（以及其他事件，例如帧缓冲区清除）的序列，列表右侧面板提供有关绘制调用的更多信息，例如几何体细节和用于渲染的着色器。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833203_73.png)
3. 逐个单步执行API调用，以便详细查看场景是如何一步步构建的。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833243_38.png) ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833251_45.png) ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833259_38.png) ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833266_44.png)

#### 渲染目标显示 <a id="%E6%B8%B2%E6%9F%93%E7%9B%AE%E6%A0%87%E6%98%BE%E7%A4%BA"></a>

* 信息面板的顶部是一个工具栏，可为 Game 视图的当前状态隔离红色、绿色、蓝色和 Alpha 通道。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833296_17.png)
* 可使用这些通道按钮右侧的 Levels 滑动条，根据亮度级别来隔离视图区域。  
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833307_60.png)  


  > 这些功能仅在渲染到 RenderTexture 时才会启用。

* 一次渲染到多个渲染目标时，可以在 Game 视图中选择显示哪个渲染目标。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833336_11.png)
* 可通过从下拉选单中选择“Depth”来查看深度缓冲区内容： ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833352_80.png)

#### 查看着色器属性值 <a id="%E6%9F%A5%E7%9C%8B%E7%9D%80%E8%89%B2%E5%99%A8%E5%B1%9E%E6%80%A7%E5%80%BC"></a>

单击“Shader Properties”选项卡即可显示属性：  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625833365_79.png)  
展示内容：

* 属性的值
* 在哪些着色器阶段 （顶点、片元、几何体、外壳、域）中使用了它
* 纹理的缩略图，单击纹理，会在项目窗口中突出显示纹理。

#### 连接手机进行远程调试 <a id="%E8%BF%9E%E6%8E%A5%E6%89%8B%E6%9C%BA%E8%BF%9B%E8%A1%8C%E8%BF%9C%E7%A8%8B%E8%B0%83%E8%AF%95"></a>

1. 手机开启开发者模式和USB调试
2. 构建时 Building Setting需要勾选Development Build，Autoconnect Profiler，Script Debugging。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626179489_75.png)
3. 直接选择 BuildAndRun会自动安装到手机并在Editor中输出。打开Frame Debug，点击下图红框处，连接手机。 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626179597_54.png)
4. 连接成功后，就和上述一样操作进行分析。

> **几点tips：**

* 如果不使用 BuildAndRun，请点击图中间的“Editor”选中“Enter IP" 输入手机设备的ip。
* 连接成功的标志：“Editor”变为：“AutoConnectPlayer”则链接成功。
* 注意：连接时，手机上的所要被调试的app一定要在运行状态！然后才能连接成功。

