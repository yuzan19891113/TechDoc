# 内存分析

## 内存简单分析 <a id="%E5%86%85%E5%AD%98%E7%AE%80%E5%8D%95%E5%88%86%E6%9E%90"></a>

### Unity Profiler Memory <a id="unity-profiler-memory"></a>

Unity自带的Profiler提供了简单的内存分析功能, 项目中默认关闭了统计内存，需要在此处打开  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626075001_71.png)  
开启后即可在Profiler中看到实时的内存统计数据，具体的信息可以在Unity的官方文档查阅 [https://docs.unity3d.com/cn/2019.4/Manual/ProfilerMemory.html](https://docs.unity3d.com/cn/2019.4/Manual/ProfilerMemory.html)

![](../../.gitbook/assets/image%20%28245%29.png)

### Perfdog <a id="perfdog"></a>

1. 在[https://perfdog.qq.com/](https://perfdog.qq.com/) 下载一个perfdog，连接手机，选择QQ飞车，在手机上启动游戏就可以看到内存统计数据。



  

2. 点击右上角的播放按钮 ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626076721_36.png)开始记录当前的统计数据，再按停止按钮可以将该区间内的统计数据上传至服务器。
3. 点击![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626077027_29.png)可以看到并且对比所有上传的统计数据

![&#x8BB0;&#x5F55;&#x4E0E;&#x4E0A;&#x4F20;&#x5185;&#x5B58;](../../.gitbook/assets/image%20%28247%29.png)

![&#x4E0D;&#x540C;Trace&#x5185;&#x5B58;&#x5BF9;&#x6BD4;](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626077127_78.png)

## 模块泄露 <a id="%E6%A8%A1%E5%9D%97%E6%B3%84%E9%9C%B2"></a>

我们游戏是一个大型的状态机，不同状态切换时会释放掉一些不应该在该状态中存在的模块，当不该在当前状态存在的模块没有被释放时，就可以认为发生了模块泄露。

由于模块之前会有相互引用，所以当一个模块泄漏时可能会钩住一串模块使其无法释放。  
例如这个案例是从大厅退出到登陆界面时报的模块泄露，  


![](../../.gitbook/assets/image%20%28240%29.png)

  
引擎中已经合入了模块泄露分析工具，开关在工具-&gt;性能，该工具可以生成pdf来帮助排查哪个模块的属性造成了模块泄露

![&#x6A21;&#x5757;&#x6CC4;&#x6F0F;&#x5F00;&#x5173;](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626088832_36.png)

  
可以看到模块泄露是被PandoraSdkMdl引发的，向上继续跟踪发现是一个Pandora对象的\_logToGameDelegate没有被置空造成的泄露，修改后即可解决

![](../../.gitbook/assets/image%20%28242%29.png)



