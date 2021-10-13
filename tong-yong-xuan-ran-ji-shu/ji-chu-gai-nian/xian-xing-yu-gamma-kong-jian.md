# 线性与gamma空间

为什么要使用线性空间？

#### 光照计算需要线性

![Left: Lighting a sphere in linear space. Right: Lighting a sphere in gamma space](https://docs.unity3d.com/uploads/Main/LinearRendering-LightingSphereLinearGamma.png)Left: Lighting a sphere in linear space. Right: Lighting a sphere 



#### 混合需要线性

![Top: Blending in linear color space produces expected blending results\<br/>Bottom: Blending in gamma color space results in over-saturated and overly-bright blends](https://docs.unity3d.com/uploads/Main/LinearRendering-BlendingLinearGamma.jpg)up: Linear space blend. Right: Gamma space blend

#### 后期算法需要线性

在unity的这个路径下Edit->Project Settings->Player->Other Settings，可以选择linear空间或者gamma空间。这两种空间会发生什么事，见下。

### 当ColorSpace选择LinearSpace时 <a href="dang-colorspace-xuan-ze-linearspace-shi" id="dang-colorspace-xuan-ze-linearspace-shi"></a>

第一张图：shader中tex2D读取颜色参与计算。 \
LinearSpace时，除非对指定图片选择了bypass sRGB，否则所有纹理都会变成sRGB格式。 \
对于sRGB的纹理，GPU会自动将colorG0（偏亮）转换到linear space，即colorL0（偏暗）。也就是说，在此转换之前，存储在纹理中的颜色colorG0是在gamma space的（偏亮）。 \


![](<../../.gitbook/assets/image (99).png>)

第二张图：shader计算结果到写入color buffer。 所有计算应该发生在linear space，

### shader 线性(没法兼容后期,只解决了光照线性）：

计算结束后需GPU会将该像素颜色再次转换到gamma space，再写入colorbuffer

![](<../../.gitbook/assets/image (97).png>)

### 全流程线性(兼容后期)

写入colorbuffer仍旧为线性，然后通过后期处理，将经过混合和后期处理后的colorbuffer转换为gamma color buffer

![](<../../.gitbook/assets/image (102).png>)

第三张图：显示器把color buffer显示到眼睛。 \
color buffer中的颜色和人眼看到的不同，这是显示器做的事，这个步骤叫display transfer，目前就掌握到这个程度。 

![](<../../.gitbook/assets/image (98).png>)

### 当ColorSpace选择gammaSpace时 <a href="dang-colorspace-xuan-ze-gammaspace-shi" id="dang-colorspace-xuan-ze-gammaspace-shi"></a>

选择gammaSpace时，整个流程中GPU不再做颜色空间转换。纹理中的颜色直接参与计算，而显示器还会正常display transfer。至于存储在纹理中的颜色colorG0应当是在什么space中，不得其解。 \
第一张图+第二张图： \


![gammaSpace时](https://img-blog.csdn.net/20150608234921084)

第三张图：显示器把color buffer显示到眼睛。 （和上面没区别） \


![](<../../.gitbook/assets/image (101).png>)

## 对于unity 材质color会隐式的转线性，特效color不会处理 <a href="zui-hou" id="zui-hou"></a>

HDR == float rt
