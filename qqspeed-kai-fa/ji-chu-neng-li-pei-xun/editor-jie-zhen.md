# Editor 截帧

## 简单介绍

\*\*帧调试器 (Frame Debugger) 可将正在运行的游戏的状态冻结到特定帧来自由回放，并查看用于渲染该帧的各个\_绘制调用\_。除了列出绘制调用，调试器还可逐个单步执行这些调用，以便您详细查看场景是如何从场景的图形元素构建的。

## 工具使用

1. 打开Frame Debugger 窗口，菜单栏中 Window --> Analysis --> \__Frame Debugger\_\_。
2. 点击下图红框处，变为"Disbale"，就会显示绘制调用信息。\
   主列表以层级视图形式显示绘制调用（以及其他事件，例如帧缓冲区清除）的序列，列表右侧面板提供有关绘制调用的更多信息，例如几何体细节和用于渲染的着色器。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833203\_73.png)
3. 逐个单步执行API调用，以便详细查看场景是如何一步步构建的。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833243\_38.png)\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833251\_45.png)\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833259\_38.png)\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833266\_44.png)

#### 渲染目标显示 <a href="e6-b8-b2-e6-9f-93-e7-9b-ae-e6-a0-87-e6-98-be-e7-a4-ba" id="e6-b8-b2-e6-9f-93-e7-9b-ae-e6-a0-87-e6-98-be-e7-a4-ba"></a>

* 信息面板的顶部是一个工具栏，可为 Game 视图的当前状态隔离红色、绿色、蓝色和 Alpha 通道。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833296\_17.png)
*   可使用这些通道按钮右侧的 Levels 滑动条，根据亮度级别来隔离视图区域。\
    ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833307\_60.png)\


    > 这些功能仅在渲染到 RenderTexture 时才会启用。
* 一次渲染到多个渲染目标时，可以在 Game 视图中选择显示哪个渲染目标。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833336\_11.png)
* 可通过从下拉选单中选择“Depth”来查看深度缓冲区内容：\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833352\_80.png)

#### 查看着色器属性值 <a href="e6-9f-a5-e7-9c-8b-e7-9d-80-e8-89-b2-e5-99-a8-e5-b1-9e-e6-80-a7-e5-80-bc" id="e6-9f-a5-e7-9c-8b-e7-9d-80-e8-89-b2-e5-99-a8-e5-b1-9e-e6-80-a7-e5-80-bc"></a>

单击“Shader Properties”选项卡即可显示属性：\
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625833365\_79.png)\
展示内容：

* 属性的值
* 在哪些着色器阶段 （顶点、片元、几何体、外壳、域）中使用了它
* 纹理的缩略图，单击纹理，会在项目窗口中突出显示纹理。

#### 连接手机进行远程调试 <a href="e8-bf-9e-e6-8e-a5-e6-89-8b-e6-9c-ba-e8-bf-9b-e8-a1-8c-e8-bf-9c-e7-a8-8b-e8-b0-83-e8-af-95" id="e8-bf-9e-e6-8e-a5-e6-89-8b-e6-9c-ba-e8-bf-9b-e8-a1-8c-e8-bf-9c-e7-a8-8b-e8-b0-83-e8-af-95"></a>

1. 手机开启开发者模式和USB调试
2. 构建时 Building Setting需要勾选Development Build，Autoconnect Profiler，Script Debugging。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1626179489\_75.png)
3. 直接选择 BuildAndRun会自动安装到手机并在Editor中输出。打开Frame Debug，点击下图红框处，连接手机。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1626179597\_54.png)
4. 连接成功后，就和上述一样操作进行分析。

> **几点tips：**

* 如果不使用 BuildAndRun，请点击图中间的“Editor”选中“Enter IP" 输入手机设备的ip。
* 连接成功的标志：“Editor”变为：“AutoConnectPlayer”则链接成功。
* 注意：连接时，手机上的所要被调试的app一定要在运行状态！然后才能连接成功。
