# 标准实时阴影

#### Standard Shadow Mapping：

的基本思想是在光源位置放置一个相机\(Light space Camera\)，画一遍深度得到深度图，在渲染场景时将pixel坐标转换Light Space计算深度，然后比较它深度和深度图中的深度，如果比深度图中深度大就意味着在阴影中，否则在被照亮。

点光源，spot使用透视矩阵投影，方向光使用正交投影。

  
阴影的锯齿有两类：透视导致的锯齿\(Perspective alias\)和投影导致的锯齿（Project alias）。  
2.PCF：投影导致的锯齿是因为灯光投射方向和物体表面夹角过小时多pixel对应阴影图的一个texel，这可以通过提高阴影图的大小来解决，也可以通过Percentage Closer Filtering来柔化边缘。PCF就是在绘制时，除了绘制当前点还会对周围像素进行多次采样、混合来柔化锯齿，常用PCF有：[使用随机采样实现soft shadow](https://link.zhihu.com/?target=http%3A//blog.csdn.net/candycat1992/article/details/8981370)、[泊松采样](https://link.zhihu.com/?target=http%3A//www.ownself.org/blog/2010/percentage-closer-filtering.html)等。多次采样目标位置附近的texel，每次获得的结果为shadowStrength在阴影中，1不在阴影中，然后把所有结果混合可以得出\[shadowStrength, 1\]之间的值，用于决定该处阴影的强度。

 

#### CSM/PSSM（Cascaded shadow mapping\):

这是两种分别研究发表但是原理几乎一样的阴影技术，Unity用的就是CSM，而其中PSSM是几个中国人（Zhang F, Sun H Q, Xu L L, et al，[观摩大佬风采](https://link.zhihu.com/?target=http%3A//cs.scu.edu.cn/cs/xyxw/webinfo/2014/05/1397522799914515.htm)）提出的。它们的原理如下：  
a\)对摄像机视锥体内沿着Z由近到远切阴影图分为多张，而切分是两种切分规则的混合，一种是均匀切分，一种是指数切分，两者按照一定比率混合起来。  
![](https://pic2.zhimg.com/80/v2-0ae72a1c96b4279b6b085e00c26f57ad_720w.png)  
  


![CSM&#x5212;&#x5206;](../../.gitbook/assets/image%20%2875%29.png)

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
Unity5内置的阴影的实现方式是Screen Space Shadow Mapping，流程如图

 ![](http://km.oa.com/files/photos/pictures/201707/1499402620_97_w292_h178.png)![](http://km.oa.com/files/photos/pictures/201707/1499402620_17_w292_h178.png)![](http://km.oa.com/files/photos/pictures/201707/1499402620_56_w292_h218.png) ![](http://km.oa.com/files/photos/pictures/201707/1499402620_74_w292_h173.png)![](http://km.oa.com/articles/show/329935?from=iSearch)  ![](http://km.oa.com/articles/show/329935?from=iSearch)

使用了三张深度图，每个shadow caster都需要被多渲染三次，对手游来说开销无法接受。**即使使用简化的Shadow Mapping，不考虑overdraw和滤波，每个shadow caster也至少要增加一个drawcall**，对同屏物体较多的游戏渲染压力还是很大，而且**需要设备支持depth texture**。

LISPSM  
在PSM的基础上又有了新的阴影技术Light Space Perspective Shadow Maps，它是在和灯光方向垂直的方向构建View Frustrum，然后将灯光、场景都转到这个View Frustrum的Perspective space，然后再计算Shadow Map，这样无论是点光、聚光、平行光就都转为平行光。  
![](http://uwa-ducument-img.oss-cn-beijing.aliyuncs.com/Blog/usparkle_shadow/14.png)

左图是Uniform（近处精度不足），中间是LISPSM（近处、远处都不错），右面是PSM（远处精度不足）。LISPSM具体细节参考：  
[https://www.cg.tuwien.ac.at/research/vr/lispsm/shadows\_egsr2004\_revised.pdf](https://www.cg.tuwien.ac.at/research/vr/lispsm/shadows_egsr2004_revised.pdf)

#### VSM （Variance ShadowMap） <a id="h3-5-3-vsm-variance-shadowmap-"></a>

算法流程与标准ShadowMap不同的是，第一个pass除了写入深度，还写入深度的平方，所以需要多一个通道来保存数据。接着对ShadowMap这张贴图进行blur，blur过程就是每个texel由其自己和周围一定范围的texel平均而得，处理完得到一张 \[E\(Depth\), E\(Depth\)2\] 的贴图，于是方差可以根据方差公式 V\(Depth\)=E\(Depth2\)-E\(Depth\)2 计算得到。

切比雪夫不等式如下图所示，它可以得出x&gt;=t的概率上限。在进行深度比较时，以往得到的是该fragment是否在阴影中，VSM计算出来的则是该fragment在阴影中的概率。这里的概率直接使用概率上限来近似，代入的t为fragment实际距离光源的深度。得到了概率，就可以用它来代表阴影强度，计算不同灰阶的阴影，从而实现软阴影。

**它的缺点第一个是需要两个通道来纪录深度信息，有额外的开销。**



