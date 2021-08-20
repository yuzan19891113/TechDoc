# 日志

## 加日志

Logger.Log\(Logger.XX\("\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description{0},{1}"\).Append\(Param0}\)

Logger.Log\(Logger.XX\(string.Format\("\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description{0}",msg\)\)

禁止使用字符串如下直接拼接的做法，参见[二进制日志思路](https://iwiki.woa.com/pages/viewpage.action?pageId=770477074)

Logger.DebugLog\("\[PracticeMgr\]\[tnqiang\]mOneKeyDownloadVideoLst: " + mOneKeyDownloadVideoLst.Count\)

### 提示日志\(提示游戏关键流程\)

Logger.log\(log.Hint\(""\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description.""\)\)

```text
Logger.Log(Logger.Hint("[AvatarAiMdl][TrusteeshipComponent] TrusteeshipMainActor{0}.").Append(Actor.getname());
```

### 增强日志\(调试分析，默认不输出\)

Logger.log\(log.Debug\(""\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description.""\)\)：一般需要开启**EnableDebugLog**

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-20_11-20-9.png?version=1&modificationDate=1629429610000&api=v2)![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-20_11-20-27.png?version=1&modificationDate=1629429628000&api=v2)

```text
 if(FixEnableMgr.EnableDebugLog)  Logger.Log(Logger.Debug("[PetUIMgr][Pet3DSceneMgr]CheckPetClick()")
```

### 警告日志\(需要清理的报错\)

Logger.log\(log.Warn\(""\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description.""\)\)

```text
Logger.Log(Logger.Warn("[SkeletonRagdoll][SkeletonRagdoll]Can't call SetSkeletonPosition while Ragdoll is not active!")
```

### 错误日志\(需要立即解决的报错\)

Logger.log\(log.Error\(""\[[ModuleName](https://iwiki.woa.com/pages/createpage.action?spaceKey=QSpeed&title=ModuleName&linkCreation=true&fromPageId=889945895)\]\[ClassName\]Log Description.""\)\)

```text
Logger.Log(Logger.Error("[ActivityMdl][SuperInductionSystem] Can not find lottery task! taskid={0}").Append(taskId));
```

### Xlua加日志\(适用于外网添加日志或者热填加日志\)

CS.Logger.XXLog\("Log Description"..Param\)

```text
CS.Logger.ErrorLog("cfg day:"..cfg.dwDayIndex);
```

## 提取日志

### 编辑器日志

####  逻辑日志\(NssLog\)

console窗口查看，依次为提示日志，警告日志，错误日志

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-13_21-52-44.png?version=1&modificationDate=1628862764000&api=v2)

####  引擎日志\(需要查看引擎奔溃，卡死问题 EditorLog）

c:users/usename/AppData/Local/Unity/Editor/Editor2019.log

参考示例本地目录C:\Users\sdzanyu\AppData\Local\Unity\Editor\Editor2019.log

### 真机日志

#### 获取逻辑日志（NssLog \)

**实时日志\(复杂度低，适用于需要实时查看日志的场合\)**

Debug包：

* 直接在Debug按钮的报错窗口（仅能查看Error级别的日志）

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-20_10-40-2.png?version=1&modificationDate=1629427202000&api=v2)

* 使用Unity editor连接设备，然后可以在console窗口查看到所有日志，如果连接安卓设备，查看[http://tapd.oa.com/nssgame/app\_for\_project/eKyp3YAn](http://tapd.oa.com/nssgame/app_for_project/eKyp3YAn)\(复杂度较高，仅适用于查看实时的非Error日志\)

Publish包：

* 注销状态下，使用金手指1332后，点击其他，然后点击LogView，再点击Debug包的窗口，可以看到实时日志\(仅error 日志）

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_18-39-47.png?version=1&modificationDate=1629369587000&api=v2)![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-20_10-31-45.png?version=1&modificationDate=1629426706000&api=v2)![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-20_10-49-29.png?version=1&modificationDate=1629427770000&api=v2)

**离线日志\(复杂度中，适用于能拿到设备调试的场合\)**

Android：

1. 手机连接pc, 选择传输文件模式 Android\data\com.tencent.tmgp.speedmobile\files\NssLog或者使用[GetNssLog.bat](https://iwiki.woa.com/download/attachments/889945895/GetNssLog.bat?version=3&modificationDate=1629430182000&api=v2)\(适用Debug,publish包\)

ios：

* 手机连接pc, 打开爱思助手或者itools,通过浏览打开应用的Document（仅适用于Debug包\)

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_17-47-7.png?version=1&modificationDate=1629366427000&api=v2)![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_17-46-37.png?version=1&modificationDate=1629366398000&api=v2)

**玩家上报或者在线上报\(复杂度高，适用于玩家，ios publish包或者其他要求设备介入低的场合\)**

在自助修复中点击日志上报，会将设备最近三次nsslog和apollo log压缩成一个 openid+date.zip，并上报到腾讯云Logs2020目录中，其中nsslog不包括当前正在写的日志（正在写入的文件无法zip读取）

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_18-41-23.png?version=1&modificationDate=1629369684000&api=v2)

提取NssLog参见[http://tapd.oa.com/nssgame/markdown\_wikis/show/\#1210124081001417889](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001417889)

![](http://tapd.oa.com/tfl/captures/2018-12/tapd_10124081_base64_1544926063_41.png)

**二进制日志还原**

编辑器菜单Tools-&gt;LoggerWnd→Open Log File，

   原始日志

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_17-43-47.png?version=1&modificationDate=1629366228000&api=v2)

解析的日志

![](https://iwiki.woa.com/download/attachments/889945895/image2021-8-19_17-44-13.png?version=1&modificationDate=1629366253000&api=v2)

#### 单局复盘日志Dump\(单局录像，适用于单局需要步骤复现的bug）

**本地获取：**

![enter image description here](http://tapd.oa.com/tfl/pictures/202103/tapd_10124081_1617104921_79.png)

* PC储存路径：NssUnityProj\Cache\MatchDumpFiles
* 真机储存路径：Android\data\com.tencent.tmgp.speedmobile\files\MatchDumpFiles

**玩家上报或者在线上报**

参见[http://tapd.oa.com/nssgame/markdown\_wikis/show/\#1210124081001600749](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001600749)

**获取设备日志\(卡死，奔溃等\)**

Android\(使用adb logcat, adb bugreport的方法\)

卡死，奔溃：[GetAndoridLog.bat](https://iwiki.woa.com/download/attachments/889945895/GetAndoridLog.bat?version=1&modificationDate=1629430237000&api=v2) 

系统无响应：[GetANRLog.bat](https://iwiki.woa.com/download/attachments/889945895/GetANRLog.bat?version=1&modificationDate=1629430347000&api=v2)

[http://tapd.oa.com/nssgame/markdown\_wikis/show/\#1210124081001415253](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001415253)

IOS:  

itools里的“工具箱”点击“崩溃日志”

####  其他日志相关

* 后台无感知提取在线玩家日志 [http://tapd.oa.com/nssgame/markdown\_wikis/show/\#1210124081001858023](http://tapd.oa.com/nssgame/markdown_wikis/show/#1210124081001858023)

