# Snapdragon Profiler

## 环境准备：

Snapdragon profiler  url：[https://developer.qualcomm.com/software/snapdragon-profiler](https://developer.qualcomm.com/software/snapdragon-profiler)

&#x20;      ADB url：[https://developer.android.com/studio/releases/platform-tools](https://developer.android.com/studio/releases/platform-tools)

&#x20;      高通的手机：一加5

&#x20;      PC： windows 10

Snapdragon 配置

![](<../../.gitbook/assets/image (185).png>)

需要关闭Unity等影响adb接口的软件

### 介绍： &#x20;

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286455\_61.png)

1\.     Connect device：打开手机的开发者模式，连接设备

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286466\_6.png)

2\.     Start Page包含的功能介绍：

Ø  RealTime：手机实时的性能数据

Ø  New Trace Capture：以时间线为轴抓取调用信息

Ø  New Snapshot Capture：截帧分析

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286477\_70.png)

### 功能使用：

**1.     RealTime**

realtime展示的是当前手机的实时性能数据，分进程和系统两块，展示帧率、内存、cpu占用、网络、GPU和电量等；

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286493\_67.png)

**2.     New Trace Capture**

new trace capture可以抓取一段时间内的调用信息，先选择包，再选择要抓取的信息，CPU Frequency， CPU Idle， CPU Load等，

选完之后点Start Capture即可开始抓取，点击Stop Capture即可结束，得到相关数据展示

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286502\_39.png)

**3.     New Snapshot Capture 截帧**

a.     选择要截取的包名，然后在要截取的时机点Take Snapshot

* 双击点击查看pixel history，找到对应的draw call ，分析shader, texture , vb, ib ,cb
* 双击drawcall可以只draw到当前drawcall

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286519\_69.png)

**贴图分析**

![查看贴图slot, 贴图resource id](<../../.gitbook/assets/image (189).png>)

![贴图resource id](<../../.gitbook/assets/image (183).png>)

![unity shader 贴图默认 while](<../../.gitbook/assets/image (186).png>)

![texture slot 0-6](<../../.gitbook/assets/image (187).png>)

![](<../../.gitbook/assets/image (188).png>)

如果只有glactivetexture没有glbindTexture,那是opengl认为不需要切换贴图，可以查看以前的DC对应slot查看slot绑定的贴图

可以查看gpu实时的shader代码，比较与本地差异



b.     遇到如下Program 为Binary的解决方法：

在Options—System—GL General Options---Disable Binary改为true，然后！！！重启app！！！再次截帧

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286528\_10.png)

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286535\_39.png)

c． **修改shader**：双击下方glDrawElements查看相应dc，点开找到对应的program，选择FS进行修改，改好之后点击Apply即可在手机上生效；

详细操作步骤如下：

1\)      双击查看当前对应的dc：当前为一些UI相关的

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286567\_10.png)

2\)      找到对应的program ID，点FS进行修改，把最终结果改为vec3(1, 0, 0)，改完之后点击Apply生效

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286573\_80.png)

3\)      查看Apply生效后的效果：

![](http://tapd.oa.com/tfl/captures/2018-10/tapd\_10124081\_base64\_1540286596\_88.png)

4\)       点击Revert可以进行回退修改

ps: 有什么问题可以微信联系chancesong\
