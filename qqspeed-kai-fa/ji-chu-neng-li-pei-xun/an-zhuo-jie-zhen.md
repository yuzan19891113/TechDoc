# 安卓截帧

在此介绍两种常用的对安卓手机进行截帧的工具：Snapdragon Profiler 和Renderdoc 。

## Snapdragon Profiler

### 软件介绍

Snapdragon Profiler（骁龙分析器）是一款性能分析软件，在Windows、 Mac、和 Linux平台上都可以运行，主要是用来分析使用了高通骁龙处理器的Android设备。\
Snapdragon Profiler通过USB连接Android设备，开发者可以用来分析CPU、GPU、DSP、memory、 power、thermal（散热）和网络数据等的性能情况，找出相关的性能瓶颈并进行优化改善。

### 软件安装

下载地址：[https://developer.qualcomm.com/software/snapdragon-profiler](https://developer.qualcomm.com/software/snapdragon-profiler)

### 软件使用

> YouTube教学视频参考：[https://developer.qualcomm.com/software/snapdragon-profiler](https://developer.qualcomm.com/software/snapdragon-profiler)

### **连接手机**

注意事项：

* 先别打开要调试的游戏，不然可能没法截桢或者找到该应用。有要调试的游戏进程也先把进程杀掉，开始截桢的时候再打开。
* 记得手机设置成 **开发者模式并打开USB调试** 进行调试，否则会无法连接上profiler。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487584\_46.png)\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487605\_93.png)\
  设备连接成功后，在页面左侧之前灰色的3个选项被激活，分别对应的是：
* 实时性能追踪
* 时间线抓取分析
* 图形库的抓帧分析（暂时貌似只支持es3）

#### **1）实时性能数据（RealTime）**

RealTime模式提供了设备的实时数据，可点击具体的app来获取app的性能指标，也可直接获取整个系统的性能指标。分别在Process和System条目下选择app和系统要测试的性能指标，例如：

* app的cpu、gpu方面的开销；
* 系统的cpu、gpu、网络、电量、温度方面的数据\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487672\_92.png)\
  如果我们想对数据进行进一步的分析，可点击左上角的数据导出图标，将当前profiler的数据导出为csv文件。

#### **2）时间线抓取分析（Trace Capture）**

除了可以查看实时数据之外，还可抓取一段时间内的调用追踪，以时间线的形式展示。可视化内核和系统事件，分析CPU，GPU和DSP上的低级系统事件，查看CPU、GPU哪里费时。

* 开启一个Trace Capture：在StartPage点击New Trace Capture或者使用Capture菜单内的New Trace即可创建一个新的Trace。\
  和RealTime类似，也可以分别为app和系统选择我们感兴趣的性能指标进行检测，勾选相应的指标后，点击左上角的“Start Capture”开始抓取。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625824345\_98.png)
* 几秒种后再点击Stop Capture结束这次抓取，就获取了这1～2秒内的调用追踪，并以时间线的形式展示。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625824417\_99.png)

#### **3）抓帧分析（Snapshot Capture）**

> 参考文档：[https://zhuanlan.zhihu.com/p/339167035](https://zhuanlan.zhihu.com/p/339167035)

点击"Take Snapshot"按钮，等待数据传送完毕后，当前帧的图形库渲染的各种数据就展示出来了。该部分的主要功能：

* 单步执行并重现渲染时候的每帧调用绘制；
* 查看和编辑着色器shader并预览设备上的实现效果；
* 查看和调试像素的历史记录（仅限Opengl ES）；
* 每个DrawCall调用捕获并查看GPU指标。（需要具有Qualcomm Snapdragon 805（或更高版本）的处理器和Android 6.0（或更高版本）。\
  ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487726\_32.png)

## Renderdoc

### 软件介绍

RenderDoc是一个基于frame-capture的图形调试器，目前可用于Vulkan、D3D11、D3D12、OpenGL以及windows7-10、Linux、Android、Stadia和任天堂交换机上的OpenGL ES开发。可集成到unity或Unreal引擎中。

### 软件安装

官方下载地址：[https://renderdoc.org/](https://renderdoc.org)

### 软件使用

> 参考文档：[Renderdoc官方使用手册](https://renderdoc.org/docs/getting_started/quick_start.html)\
> \
> \#### apk准备

1. 准备好需要调试的apk

* Q飞-蓝盾流水线下载：[http://devops.oa.com/console/pipeline/speedmobile/pe12bc348783641c7aee6a56cd2747f72/history](http://devops.oa.com/console/pipeline/speedmobile/pe12bc348783641c7aee6a56cd2747f72/history)\
  在“查看构件”中下载生成好的apk包\


1. pc端安装adb工具

* 下载 `\\tencent.com\tfs\跨部门项目\QQ飞车手游\构建归档\BuildTools\Windows` 文件目录下的 `android_sdk`
* `adb.exe` 在 `\android_sdk\android_sdk\platform-tools` 下，需要将相应路径添加到系统变量的Path中，才可使用adb命令。\


1. 通过pc对手机进行apk包的安装

* win+r，输入cmd，打开“命令提示符”窗口\
  \- adb devices 检查手机和电脑是否成功连接
* adb install -r（将apk安装包直接拖进cmd里的光标位置即可），点击回车键
* 在手机上点击“允许安装应用”

### **RenderDoc连接手机**

1. 点击左下方图标，如图所示则连接成功。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487996\_53.png)
2. 打开手机上的apk文件\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625488055\_25.png)
3. 中间可能会有弹窗报错，此时要记得指定Android_sdk和Java JDK的指定路径。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625488105\_78.png)
4. 此时还可能出现下图Warning，点击确认修复。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625488134\_45.png)
5. 安卓手机上会出现弹框，点击“继续安装”。
6. 安装成功后，点击“Launch”，就可以成功连接手机。接下来就可对apk包进行截帧分析了。\
   ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625488229\_59.png)

> 如果遇到签名问题，可使用本项目中优化签名后的Renderdoc。\
> **使用方法：** 在 `\android_sdk\android_sdk\platform-tools` 目录下创建 `KeyStore` 文件夹，并放入xxx.keystore 文件，以及存放对应密码的xxx.txt 文件。就可用指定的证书文件对apk进行签名。\
> 如果找不到该文件夹，用户未指定签名证书的话，则会直接使用Renderdoc的自签名。

> renderdoc签名相关的详细文档在这里：[http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001997245](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001997245)

#### 截帧分析 <a href="e6-88-aa-e5-b8-a7-e5-88-86-e6-9e-90" id="e6-88-aa-e5-b8-a7-e5-88-86-e6-9e-90"></a>

参考文章推荐：

* [https://km.woa.com/articles/show/386953?kmref=search\&from_page=1\&no=2](https://km.woa.com/articles/show/386953?kmref=search\&from_page=1\&no=2)
* [http://www.manew.com/thread-92382-1-1.html](http://www.manew.com/thread-92382-1-1.html)
