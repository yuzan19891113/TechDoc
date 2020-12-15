# Fake real shadow

#### 地面云阴影：

对于地面上云阴影，用实时灯光照射出阴影显然是不划算，可以直接在地面shader中混合一个运动的云图就能达到类似效果。

#### 植物摇曳阴影：

 ****对于树、草、旗子这类位置不变但有摇曳动画的物体，可以预先把阴影烘焙到贴图中，然后把阴影图作为单独贴图、或地面贴图Alpha通道传送到地面shader中，然后只需要添加阴影晃动的特性的就可以，随植物晃动而晃动，会有一种真实阴影的感觉。另外注意阴影的方向、和植物晃动的同步登细节**。**

 **结合Projector和Rendertexture的实时阴影**

创建一个跟随主相机的阴影相机，改为正交投影，设置单独的shadow Layer,将需要投射阴影物体设置到shadow layer，为此阴影相机设置渲染目标到一个渲染纹理RTT\_Shadow。另外创建一个Projector，为它设置一个材质Mat\_Proj，并将RTT\_Shadow传到Mat\_Proj的shader中进行着色，另外为防止投影相机边缘的刺刺的长线，要设置一个阴影衰减纹理，如果需要软阴影则需要另外Blur。

 **角色脚下阴影面片**  
对于游戏中的NPC、杂兵、野怪这些非关键性角色可以直接用防止一个阴影面片来模拟阴影，当然如果地面起伏比较大可能会有穿插问题

![](http://km.oa.com/files/photos/pictures/201707/1499402769_23_w600_h400.png)![](http://km.oa.com/articles/show/329935?from=iSearch) 

简单在角色脚下加一个面片，赋上半透明阴影贴图。

优点：效率高

缺点：平面阴影，遇到起伏地形会穿帮；阴影内容固定，无法反映SkinnedMesh的变化；阴影方向固定

效率：全部阴影只有一个drawcall。 

Drawcall = 1

**面片加RenderTexture**

![](http://km.oa.com/files/photos/pictures/201707/1499402793_76_w600_h400.png)

在角色脚下加一个面片，赋上半透明材质，增加相机沿光源方向渲染阴影到RenderTexture，RenderTexure赋到面片材质

优点：细节精确

缺点：平面阴影，增加内存消耗

效率：每个shadow caster增加渲染RenderTexture和面片的两个drawcall。

Drawcall = shadow caster \* 2

