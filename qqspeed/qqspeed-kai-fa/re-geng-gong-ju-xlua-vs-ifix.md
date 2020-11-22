# 热更工具xlua vs IFix

## XLua

c\#热更工具xlua，可以不修改原生c\#代码的情况下，通过xlua注入dll的内部，修改dll内部的函数，ILspy反编译工具可以查看dll的函数，因为c\#极易被反编译，所以使用Dotfuscator来混淆代码，防止反编译。 xlua原理： 使用Mono.Cecil为打\[Hotfix\]标签的类注入钩子，可以为_**所有业务型代码自动插桩**_。如果发现lua补丁，就使用反射的方法为相应钩子赋值（xlua.hotfix方法） 当执行该函数时，发现钩子不为空，就执行钩子并返回

当需要调试手机时，通过UnityEngine.Networking.PlayerConnection，playerconnection, editorconnection 可以建立起push lua到手机的简易调试框架，可以方便在手机上实时调试。EditorConnection.instance.Send

## IFix

**C\#热更新工具**

**官方文档：**

[https://github.com/Tencent/InjectFix/blob/master/Doc/quick\_start.md](https://github.com/Tencent/InjectFix/blob/master/Doc/quick_start.md)

#### 优势：

1.可以直接在c\#代码上修改bug，修改好后直接merge到主线 2.修改bug可直接使用vs调试，确定修改成功后将修改后的方法打上  \[IFix.Patch\]标签即可 3.可在真机测试补丁下过，也可以用Inject的方式在手机上测试补丁效果

#### 劣势：

1.不能hotfix构造方法

 2.不能hotfix泛型方法（但是可以随意调用）

 3.不能用作功能开发，无法增加新的type和method 4.效率与xlua相当，不适合在update中hotfix

## 使用方式

1.在切出publish分之后，直接在c\#上修改对应的bug， !!\#ff0000 修改好并且本地测试通过!! 后将修改的方法打上!!\#ff0000 \[IFix.Patch\]!!标签

```text
[IFix.Patch]
private int OnSystemInit(GameEvent e)
{
    InitModules();
    return (int)EventRet.Continue;
}
```

  
 2.修改c\#等编译完成之后，点击iFix---Patch，即可在 \Tools\TDR\_res\Databin\Client!! 目录下生成  DllName.patch.bytes!! \(比如NssMain.patch.bytes\) 这个补丁

![](../../.gitbook/assets/image%20%281%29.png)

  
 3.本地测试方式（基本不需要使用）： 在没有修改bug时，编译项目，编译成功后点击 iFix---Process Assembly-CSharp!! ，备份还未改bug的程序集

![](../../.gitbook/assets/image%20%282%29.png)

在c\#上修改bug，把修改过的方法打上 \[IFix.Patch\]!! 标签，改好以后编译成功后，点击  iFix---Patch!! ，保存生成的补丁 点击iFix---Revert Assembly-CSharp\(For test only\)!!，选择刚才备份的dll做还原

![](../../.gitbook/assets/image%20%283%29.png)

运行游戏，测试补丁是否生效，可以将Assembly-CSharp.patch.bytes改名和还原来控制是否加载补丁 也可以直接Inject在手机上测试!!  
 4.发布流程，使用蓝盾任务生成和提交patch，在124表配上对应的bytes安排发布  
 5.测试同学将DllName.patch.bytes\(每个程序集如果有patch标签，会生成patch，如果没有打patch标签，则不会生成对应程序集的patch\)刷到svr，测试通过后走外发流程

#### 注意事项（持续补充）：

1.\[IFix.Patch\]!! 标签不要合到主线上 

2.由于patch无法diff，请确保c\#代码和patch一起提交并写好注释 

3.hotfix黑名单在LiveDotNetConfig.cs文件中，如果有native相关的方法无法注入，请自行加入到黑名单中 4.lua的hotfix功能已经关闭，无法再进行hotfix。系统开发功能保持不变。 

5.iFix hotfix补丁在 !游戏启动时!! 加载， 数据档拉取完成 时重载，覆盖整个游戏生命周期 

6.要注意版本问题，在覆盖安装或者重新安装新版本时，第一次启动游戏不会加载补丁，当拉到新的数据档才会加载补丁 

7.pc上测试备份的dll保存在NssUnityProj\IFixDllBackup 中，如果不需要可以自行删除 

8.区分平台要使用NssBuild.IsAndroid\(\), NssBuild.IsIos\(\)等方法，不可使用宏，否则在非对应平台上无法生成对应代码到patch 

9.使用new string去把一个char\[\]转成string会报错，可以使用stringBuilder来处理，或者使用封装好的NString.Char2String方法来进行转换

