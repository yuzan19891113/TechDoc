# Snapdragon Profiler

## 环境准备：

Snapdragon profiler  url：[https://developer.qualcomm.com/software/snapdragon-profiler](https://developer.qualcomm.com/software/snapdragon-profiler)

       ADB url：[https://developer.android.com/studio/releases/platform-tools](https://developer.android.com/studio/releases/platform-tools)

       高通的手机：一加5

       PC： windows 10

需要关闭Unity等影响adb接口的软件

### 介绍：  

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286455_61.png)

1.     Connect device：打开手机的开发者模式，连接设备

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286466_6.png)

2.     Start Page包含的功能介绍：

Ø  RealTime：手机实时的性能数据

Ø  New Trace Capture：以时间线为轴抓取调用信息

Ø  New Snapshot Capture：截帧分析

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286477_70.png)

### 功能使用：

**1.     RealTime**

realtime展示的是当前手机的实时性能数据，分进程和系统两块，展示帧率、内存、cpu占用、网络、GPU和电量等；

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286493_67.png)

**2.     New Trace Capture**

new trace capture可以抓取一段时间内的调用信息，先选择包，再选择要抓取的信息，CPU Frequency， CPU Idle， CPU Load等，

选完之后点Start Capture即可开始抓取，点击Stop Capture即可结束，得到相关数据展示

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286502_39.png)

**3.     New Snapshot Capture 截帧**

a.     选择要截取的包名，然后在要截取的时机点Take Snapshot

* 双击点击查看pixel history，找到对应的draw call ，分析shader, texture , vb, ib ,cb

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286519_69.png)

b.     遇到如下Program 为Binary的解决方法：

在Options—System—GL General Options---Disable Binary改为true，然后！！！重启app！！！再次截帧

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286528_10.png)

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286535_39.png)

c． **修改shader**：双击下方glDrawElements查看相应dc，点开找到对应的program，选择FS进行修改，改好之后点击Apply即可在手机上生效；

详细操作步骤如下：

1\)      双击查看当前对应的dc：当前为一些UI相关的

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286567_10.png)

2\)      找到对应的program ID，点FS进行修改，把最终结果改为vec3\(1, 0, 0\)，改完之后点击Apply生效

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286573_80.png)

3\)      查看Apply生效后的效果：

![](http://tapd.oa.com/tfl/captures/2018-10/tapd_10124081_base64_1540286596_88.png)

4\)       点击Revert可以进行回退修改

ps: 有什么问题可以微信联系chancesong  


