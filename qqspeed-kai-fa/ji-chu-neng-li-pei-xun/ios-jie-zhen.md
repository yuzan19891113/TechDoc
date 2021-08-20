# ios 截帧

有两种方式进行ios截帧  
1.拿到gpu trace数据 在xcode 上replay  
2.在xcode上运行xcode projects 进行截帧

### 1.拿到gpu trace数据 在xcode 上replay <a id="1%E6%8B%BF%E5%88%B0gpu-trace%E6%95%B0%E6%8D%AE-%E5%9C%A8xcode-%E4%B8%8Areplay"></a>

#### 实现原理 <a id="%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86"></a>

实现原理可参考[https://developer.apple.com/documentation/metal/frame\_capture\_debugging\_tools/enabling\_frame\_capture](https://developer.apple.com/documentation/metal/frame_capture_debugging_tools/enabling_frame_capture) Capture GPU Traces Without Xcode部分  
[https://km.woa.com/articles/show/464908?kmref=search&from\_page=1&no=1](https://km.woa.com/articles/show/464908?kmref=search&from_page=1&no=1)

#### 项目中实现 <a id="%E9%A1%B9%E7%9B%AE%E4%B8%AD%E5%AE%9E%E7%8E%B0"></a>

项目里实现了这个工具  
具体操作流程为：  
点积Debug按钮 -&gt;点击工具按钮-&gt;点击工具按钮-&gt;点击截帧按钮-&gt;点击framecapture  
![enter image description here](https://iwiki.woa.com/download/attachments/882419345/image-1628664836931.png?version=1&modificationDate=1628664836855&api=v2)

使用爱思助手 找到q飞手游应用程序的documents （要安装debug包 不然无法打开documents目录）  
![enter image description here](https://iwiki.woa.com/download/attachments/882419345/image-1628681797549.png?version=1&modificationDate=1628681797439&api=v2)

找到framecapture 文件夹 导出gpu trace 到mac 上，使用XCode打开  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625487448_88.png)  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625487468_71.png)  
可以看每个drawcall的耗时 等等  


### 2.在xcode上运行xcode projects 进行截帧 <a id="2%E5%9C%A8xcode%E4%B8%8A%E8%BF%90%E8%A1%8Cxcode-projects-%E8%BF%9B%E8%A1%8C%E6%88%AA%E5%B8%A7"></a>

#### 获取xcode projects <a id="%E8%8E%B7%E5%8F%96xcode-projects"></a>

需要拿到xcode projects 目前似乎不太好拿到  
可参考之前的文档  
[http://tapd.oa.com/nssgame/markdown\_wikis/show/\#1210124081001423527](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001423527)

#### 可能会遇到的问题： <a id="%E5%8F%AF%E8%83%BD%E4%BC%9A%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%EF%BC%9A"></a>

**（1）could not launch**

安装程序的时候可能会报这个问题  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625487264_1.png)  
需要在设备描述文件里 添加对profile的信任

**（2）没有截帧按钮**

![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625487289_29.png)

从auto改成metal就有了  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1625487353_58.png)

