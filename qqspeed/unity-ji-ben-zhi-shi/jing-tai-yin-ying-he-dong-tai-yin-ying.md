# 静态阴影和动态阴影

静态阴影使用全局光照烘培工具烘培，例如Bakery，得到shadowMask。动态阴影通过场景挂mapshadowswitch脚本开启阴影。

#### 必要步骤检查

1场景需要动态阴影，为场景挂上脚本mapshadowswitch,指定好动态物件，静态物件。

2 如果shadowmask在unity编辑器中通道显示为灰色：解决方法，检查window-&gt;lighting setttings中的mixedlighting是不是为shadowmask，修改后可以看到shadowmask变为可用状态

![](../../.gitbook/assets/image%20%2871%29.png)

3 通过调整场景中的sun的intensity，确保当前环境参数起效了，调整可以看到场景亮度变化。

![](../../.gitbook/assets/image%20%2873%29.png)

4 bakery检查，如果发现bakery突然不是前一个设置的参数，甚至被恢复为默认参数，这时候一般是bakey的参数和场景不匹配了，删掉bakery的light,以及场景中的lightmapdata,重新挂bakery 的light脚本，重新打开bakery

5 如果烘培不出阴影，检查1,2,3是否正确

6 烘培出阴影后，如果开启了动态阴影，移动测试某个静态对象，看是否静态物件也产生了动态阴影，如果有，检查第1步，**注意，如果没有烘培，静态物件也会产生动态阴影，这是正常的**。移动测试动态对象是否动态对象阴影发生改变，确认是动态阴影。

7 如果动态物体没有产生阴影，可能是阴影距离不够，增大阴影距离

8 后续动态阴影参数调整待补充

