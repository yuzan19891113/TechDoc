# shadow

#### 地面云阴影：

对于地面上云阴影，用实时灯光照射出阴影显然是不划算，可以直接在地面shader中混合一个运动的云图就能达到类似效果。

#### 植物摇曳阴影：

 ****对于树、草、旗子这类位置不变但有摇曳动画的物体，可以预先把阴影烘焙到贴图中，然后把阴影图作为单独贴图、或地面贴图Alpha通道传送到地面shader中，然后只需要添加阴影晃动的特性的就可以，随植物晃动而晃动，会有一种真实阴影的感觉。另外注意阴影的方向、和植物晃动的同步登细节**。**

 **结合Projector和Rendertexture的实时阴影**

创建一个跟随主相机的阴影相机，改为正交投影，设置单独的shadow Layer,将需要投射阴影物体设置到shadow layer，为此阴影相机设置渲染目标到一个渲染纹理RTT\_Shadow。另外创建一个Projector，为它设置一个材质Mat\_Proj，并将RTT\_Shadow传到Mat\_Proj的shader中进行着色，另外为防止投影相机边缘的刺刺的长线，要设置一个阴影衰减纹理，如果需要软阴影则需要另外Blur。

 **角色脚下阴影面片**  
**对于游戏中的NPC、杂兵、野怪这些非关键性角色可以直接用防止一个阴影面片来模拟阴影，当然如果地面起伏比较大可能会有穿插问题**

####  Standard Shadow Mapping：

的基本思想是在光源位置放置一个相机\(Light space Camera\)，画一遍深度得到深度图，在渲染场景时将pixel坐标转换Light Space计算深度，然后比较它深度和深度图中的深度，如果比深度图中深度大就意味着在阴影中，否则在被照亮。  
阴影的锯齿有两类：透视导致的锯齿\(Perspective alias\)和投影导致的锯齿（Project alias）。  
2.PCF：投影导致的锯齿是因为灯光投射方向和物体表面夹角过小时多pixel对应阴影图的一个texel，这可以通过提高阴影图的大小来解决，也可以通过Percentage Closer Filtering来柔化边缘。PCF就是在绘制时，除了绘制当前点还会对周围像素进行多次采样、混合来柔化锯齿，常用PCF有：[使用随机采样实现soft shadow](https://link.zhihu.com/?target=http%3A//blog.csdn.net/candycat1992/article/details/8981370)、[泊松采样](https://link.zhihu.com/?target=http%3A//www.ownself.org/blog/2010/percentage-closer-filtering.html)等。

 

#### CSM/PSSM（Cascaded shadow mapping\):

这是两种分别研究发表但是原理几乎一样的阴影技术，Unity用的就是CSM，而其中PSSM是几个中国人（Zhang F, Sun H Q, Xu L L, et al，[观摩大佬风采](https://link.zhihu.com/?target=http%3A//cs.scu.edu.cn/cs/xyxw/webinfo/2014/05/1397522799914515.htm)）提出的。它们的原理如下：  
a\)对摄像机视锥体内沿着Z由近到远切阴影图分为多张，而切分是两种切分规则的混合，一种是均匀切分，一种是指数切分，两者按照一定比率混合起来。  
![](https://pic2.zhimg.com/80/v2-0ae72a1c96b4279b6b085e00c26f57ad_720w.png)  
  
b\)对每一块分别计算一个光源投影空间内平移、缩放的矩阵cropMatrix，它可以将切分的多块移动、缩放到光源的视椎中，这个矩阵和正交投影矩阵非常像。  


```text
   // Build a matrix for cropping light's projection
   // Given vectors are in light's clip space
Matrix Light::CalculateCropMatrix(Frustum splitFrustum)
{
  Matrix lightViewProjMatrix = viewMatrix * projMatrix;
  // Find boundaries in light's clip space
  BoundingBox cropBB = CreateAABB(splitFrustum.AABB,
                                  lightViewProjMatrix);
  // Use default near-plane value
  cropBB.min.z = 0.0f;
  // Create the crop matrix
  float scaleX, scaleY, scaleZ;
  float offsetX, offsetY, offsetZ;
  scaleX = 2.0f / (cropBB.max.x - cropBB.min.x);
  scaleY = 2.0f / (cropBB.max.y - cropBB.min.y);
  offsetX = -0.5f * (cropBB.max.x + cropBB.min.x) * scaleX;
  offsetY = -0.5f * (cropBB.max.y + cropBB.min.y) * scaleY;
  scaleZ = 1.0f / (cropBB.max.z - cropBB.min.z);
  offsetZ = -cropBB.min.z * scaleZ;
  return Matrix( scaleX,     0.0f,     0.0f,  0.0f,
                   0.0f,   scaleY,     0.0f,  0.0f,
                   0.0f,     0.0f,   scaleZ,  0.0f,
                offsetX,  offsetY,  offsetZ,  1.0f);
}
```

c\)针对切分的每一块渲染阴影图，一般阴影图大小一样的，比如都是1024\*1024，而近处包含的场景范围比远处小，所以近处阴影图的精度会更高。  
d\)渲染场景阴影  
![](https://pic4.zhimg.com/80/v2-0a754fdc0b823495b997af96ab509c53_720w.jpg)  


