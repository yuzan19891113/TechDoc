# 伪实时阴影

#### 地面云阴影：

对于地面上云阴影，用实时灯光照射出阴影显然是不划算，可以直接在地面shader中混合一个运动的云图就能达到类似效果。

#### 植物摇曳阴影：

 ****对于树、草、旗子这类位置不变但有摇曳动画的物体，可以预先把阴影烘焙到贴图中，然后把阴影图作为单独贴图、或地面贴图Alpha通道传送到地面shader中，然后只需要添加阴影晃动的特性的就可以，随植物晃动而晃动，会有一种真实阴影的感觉。另外注意阴影的方向、和植物晃动的同步登细节**。**

 **结合Projector和Rendertexture的实时阴影**

创建一个跟随主相机的阴影相机，改为正交投影，设置单独的shadow Layer,将需要投射阴影物体设置到shadow layer，为此阴影相机设置渲染目标到一个渲染纹理RTT\_Shadow。另外创建一个Projector，为它设置一个材质Mat\_Proj，并将RTT\_Shadow传到Mat\_Proj的shader中进行着色，另外为防止投影相机边缘的刺刺的长线，要设置一个阴影衰减纹理，如果需要软阴影则需要另外Blur。

使用replaceShader生成一张“轮廓”图，使用untiy自带的Projector组件投影到场景中，在shader中计算从而生成阴影效果。使用replaceShader是为了避免渲染“轮廓”图的时候还是用原来复杂的shader，照成不必要的性能浪费。替换后我们只输出纯色（方便测试）或者alpha值到“轮廓”图。

![](../../.gitbook/assets/image%20%2843%29.png)

![](../../.gitbook/assets/image%20%2857%29.png)

shadow caster上加光源方向的Projector，加渲染阴影到RenderTexture的相机，RenderTexture赋到Projector的材质

当“轮廓”图中的物体贴近边缘的时候阴影会产生bug，如下图，看到阴影被拉长了一条。

![](../../.gitbook/assets/image%20%2847%29.png)

这是由于“轮廓”图的wrapMode采用使用了clamp导致的，clamp模式就是uv在超过1的时候把图像边缘的像素拉伸，如下图：

如何解决呢？两个方法。

1）求包围盒的时候设置一个offset，使包围盒向外扩大一点，这样物体就不会贴着“轮廓”图的边缘了

2）在投影器的shader中使用一张图来做遮罩（下图），这样边缘的像素可以过滤掉，还可以做出边缘的阴影渐隐的效果。

![](../../.gitbook/assets/image%20%2851%29.png)

优点：投射阴影，细节精确

缺点：效率低，增加内存消耗

效率：每个shadow caster增加渲染RenderTexture的一次drawcall，每个在shadow caster投射区域的shadow receiver，都会增加一次渲染

Drawcall ≈ （shadow caster +1）\* shadow receiver

####  **角色脚下阴影面片**

  
对于游戏中的NPC、杂兵、野怪这些非关键性角色可以直接用防止一个阴影面片来模拟阴影，当然如果地面起伏比较大可能会有穿插问题

![](http://km.oa.com/files/photos/pictures/201707/1499402769_23_w600_h400.png)![](http://km.oa.com/articles/show/329935?from=iSearch) 

简单在角色脚下加一个面片，赋上半透明阴影贴图。

优点：效率高

缺点：平面阴影，遇到起伏地形会穿帮；阴影内容固定，无法反映SkinnedMesh的变化；阴影方向固定

效率：全部阴影只有一个drawcall。 

Drawcall = 1

\*\*\*\*

