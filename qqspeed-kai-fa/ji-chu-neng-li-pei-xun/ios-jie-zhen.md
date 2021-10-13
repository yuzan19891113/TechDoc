# ios 截帧

有两种方式进行ios截帧\
1.拿到gpu trace数据 在xcode 上replay\
2.在xcode上运行xcode projects 进行截帧

### 1.拿到gpu trace数据 在xcode 上replay <a href="1-e6-8b-bf-e5-88-b0gpu-trace-e6-95-b0-e6-8d-ae-e5-9c-a8xcode-e4-b8-8areplay" id="1-e6-8b-bf-e5-88-b0gpu-trace-e6-95-b0-e6-8d-ae-e5-9c-a8xcode-e4-b8-8areplay"></a>

#### 实现原理 <a href="e5-ae-9e-e7-8e-b0-e5-8e-9f-e7-90-86" id="e5-ae-9e-e7-8e-b0-e5-8e-9f-e7-90-86"></a>

实现原理可参考[https://developer.apple.com/documentation/metal/frame_capture_debugging_tools/enabling_frame_capture](https://developer.apple.com/documentation/metal/frame_capture_debugging_tools/enabling_frame_capture) Capture GPU Traces Without Xcode部分\
[https://km.woa.com/articles/show/464908?kmref=search\&from_page=1\&no=1](https://km.woa.com/articles/show/464908?kmref=search\&from_page=1\&no=1)

#### 项目中实现 <a href="e9-a1-b9-e7-9b-ae-e4-b8-ad-e5-ae-9e-e7-8e-b0" id="e9-a1-b9-e7-9b-ae-e4-b8-ad-e5-ae-9e-e7-8e-b0"></a>

项目里实现了这个工具\
具体操作流程为：\
点积Debug按钮 ->点击工具按钮->点击工具按钮->点击截帧按钮->点击framecapture\
![enter image description here](https://iwiki.woa.com/download/attachments/882419345/image-1628664836931.png?version=1\&modificationDate=1628664836855\&api=v2)

使用爱思助手 找到q飞手游应用程序的documents （要安装debug包 不然无法打开documents目录）\
![enter image description here](https://iwiki.woa.com/download/attachments/882419345/image-1628681797549.png?version=1\&modificationDate=1628681797439\&api=v2)

找到framecapture 文件夹 导出gpu trace 到mac 上，使用XCode打开\
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487448\_88.png)\
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487468\_71.png)\
可以看每个drawcall的耗时 等等\


### 2.在xcode上运行xcode projects 进行截帧 <a href="2-e5-9c-a8xcode-e4-b8-8a-e8-bf-90-e8-a1-8cxcode-projects-e8-bf-9b-e8-a1-8c-e6-88-aa-e5-b8-a7" id="2-e5-9c-a8xcode-e4-b8-8a-e8-bf-90-e8-a1-8cxcode-projects-e8-bf-9b-e8-a1-8c-e6-88-aa-e5-b8-a7"></a>

#### 获取xcode projects <a href="e8-8e-b7-e5-8f-96xcode-projects" id="e8-8e-b7-e5-8f-96xcode-projects"></a>

需要拿到xcode projects 目前似乎不太好拿到\
可参考之前的文档\
[http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001423527](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001423527)

#### 可能会遇到的问题： <a href="e5-8f-af-e8-83-bd-e4-bc-9a-e9-81-87-e5-88-b0-e7-9a-84-e9-97-ae-e9-a2-98-ef-bc-9a" id="e5-8f-af-e8-83-bd-e4-bc-9a-e9-81-87-e5-88-b0-e7-9a-84-e9-97-ae-e9-a2-98-ef-bc-9a"></a>

**（1）could not launch**

安装程序的时候可能会报这个问题\
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487264\_1.png)\
需要在设备描述文件里 添加对profile的信任

**（2）没有截帧按钮**

![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487289\_29.png)

从auto改成metal就有了\
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd\_10124081\_1625487353\_58.png)
