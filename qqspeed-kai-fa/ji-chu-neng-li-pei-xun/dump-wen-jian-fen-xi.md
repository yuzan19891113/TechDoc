# Dump文件分析

Dump文件分析用于解决当发生 Crash 的时候，能够根据系统的 Dump 文件或者日志还原出崩溃堆栈，从而方便解决崩溃问题。

## **Android本地还原**

![](https://iwiki.woa.com/plugins/servlet/view-file-macro/placeholder?type=unknown&name=auto_symbolicate_android.py&attachmentId=923329485&version=1&mimeType=application%2Foctet-stream&height=250)

### 将手机连上电脑，使用 adb 查看系统日志；

### 找到崩溃日志，将崩溃堆栈（只有地址信息）复制，保存到一个 crash\_stack.txt 中，例如：![](https://iwiki.woa.com/download/attachments/923329294/image2021-8-4_16-26-51.png?version=1&modificationDate=1628065611000&api=v2)

### 通过蓝盾的制品库，找到对应版本的符号文件，下载到本地；

### 下载上面的 auto\_symbolicate\_android.py, 然后把下载的符号文件解压到同一个目录，crash\_stack.txt 也放到同一个目录；

### 执行 auto\_symbolicate\_android.py 即可生成堆栈。

## **iOS 构建机还原** 

### 手机上拿 dump 文件

点击：设置\隐私\分析与改进，里面可以看到所有应用的崩溃记录，根据包名和时间即可获取。

###  上传 ips 文件

repo 地址：[http://tc-svn.tencent.com/ied/ied\_nssclient\_rep/nssclient\_proj/branches/CrashRepo](http://tc-svn.tencent.com/ied/ied_nssclient_rep/nssclient_proj/branches/CrashRepo)

###  启动构建任务，填入 ips 文件 svn 地址和版本号等参数

构建任务地址：[http://devops.oa.com/console/pipeline/speedmobile/p-70fae98e70894875a8372ec4fa11d684/history](http://devops.oa.com/console/pipeline/speedmobile/p-70fae98e70894875a8372ec4fa11d684/history)

![](https://iwiki.woa.com/download/attachments/923329294/image2021-8-12_14-38-29.png?version=1&modificationDate=1628750310000&api=v2)

## **iOS 本地还原**

**如果本地有 mac 电脑，可以使用本地还原的方式**

### 手机上拿 dump 文件

点击：设置\隐私\分析与改进，里面可以看到所有应用的崩溃记录，根据包名和时间即可获取。

### 还原堆栈

#### 在桌面新建一个文件夹，名字叫crash

#### 将.ips文件更名为.crash文件并放到crash文件夹中

#### 前往文件夹路径  /应用程序/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources复制 symbolicatecrash 脚本并粘贴到crash文件夹中

#### 复制xxx.app.dSYM文件粘贴到crash文件夹中

#### 打开终端输入命令：

cd/Users/example/Desktop/crash  //进入到桌面crash文件夹中

./symbolicatecrash /Users/example/Desktop/crash/59129929.crash/Users/example/Desktop/crash/xxx.app.dSYM &gt; log.crash //进行crash日志解析

### 注意： 如果终端报错：

Error: "DEVELOPER\_DIR" is not defined at ./symbolicatecrash line 69.

输入：

exportDEVELOPER\_DIR="/Applications/XCode.app/Contents/Developer"

### 参考 [https://www.jianshu.com/p/5f88a0bb2956](https://www.jianshu.com/p/5f88a0bb2956)

