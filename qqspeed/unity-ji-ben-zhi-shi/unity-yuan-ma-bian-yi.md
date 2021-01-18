# Unity 源码编译

源码版本：Unity2017.4.17f1

### Windows篇 <a id="toc0"></a>

1、 Windows 7 SP1 64位以上，同时需要VS 2013和VS 2015 的CRT DLL \(一般都装过了\)  
2、 Perl，安装ActivePerl 5.22或更高版本，并确保Perl位于%PATH%.  
3、 VisualStudio 2010\(必须安装Service Pack 1\).  
a\) 如果未安装SP1，会报类似错误\_XCR\_FEATURE\_ENABLED\_MASK  
4、 也可以使用2015 Update 2 及以上版本

如果只需要构建WindowsEditor，不需要Android sdk&ndk。

Android sdk&ndk可以去IFS上拷贝：  
![&#x56FE;&#x7247;&#x63CF;&#x8FF0;](http://tapd.oa.com/tfl/captures/2019-04/tapd_10124081_base64_1554864051_20.png)  
\tencent.com\tfs\跨部门项目\QQ飞车手游\构建归档\BuildTools

**拉下源码之后cmd中执行**

```text
perl build.pl --prepare 
```

### Mac篇 <a id="toc1"></a>

#### 拉取Unity2017源码 <a id="toc2"></a>

[http://tc-svn.tencent.com/qqspeed/nssunity5\_proj/trunk/Unity2017.4.17f1](http://tc-svn.tencent.com/qqspeed/nssunity5_proj/trunk/Unity2017.4.17f1)

#### 配置xcode <a id="toc3"></a>

确认xcode有以下sdk:

* /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk
* /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS10.0.sdk
* /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator10.0.sdk

没有的话去\[TFS目录\] : \tencent.com\tfs\跨部门项目\QQ飞车手游\构建归档\BuildTools 上拷贝。  
![&#x56FE;&#x7247;&#x63CF;&#x8FF0;](http://tapd.oa.com/tfl/captures/2019-04/tapd_10124081_base64_1554864051_20.png)

#### 配置Android sdk & ndk <a id="toc4"></a>

确认是否有安卓SDK 23，和ndk r13b。 没有的话去\[TFS目录\] : \tencent.com\tfs\跨部门项目\QQ飞车手游\构建归档\BuildTools 上拷贝。

**目录注意去掉 - ，都换成\_**

#### 构建命令 <a id="toc5"></a>

```text
$ # 下面的android sdk & ndk路径换成你本机上的路径$ export ANDROID_SDK_ROOT=/Users/tencent/NssGameBuildTools/android_sdk$ export ANDROID_NDK_ROOT=/Users/tencent/NssGameBuildTools/android_ndk_r13b$ cd path/to/Unity2017Src$ python NGameBuild_MacEngine.py$ # 或者手动输入unity的构建命令
```

#### FAQ <a id="toc6"></a>

执行perl build.pl --prepare 报错  
![&#x56FE;&#x7247;&#x63CF;&#x8FF0;](http://tapd.oa.com/tfl/captures/2020-06/tapd_10124081_base64_1591169833_92.png)

删掉artifacts, build 目录重新执行（或者执行 perl build.pl --reset）

