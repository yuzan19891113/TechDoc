# 糖豆车实时阴影方案

### 应用场景

糖豆车若干动态物件的实时阴影。

~~低配看时间，如果后期时间有余量，可以考虑优化projector的方法，但优先保证中高配的实时阴影\(参考和平精英，codm中低配都没阴影\)~~

### 阴影配置

建议shadowmap1024 \* 1024  shadowdistance\(150-200\),程序再根据配置来动态调整

### 阴影技术方案：

**单张shadowmap + PCF soft**

**单张shadowmap:** 1024 \* 1024\(medium\),  小于shadow distance（150- 200）的 动态物件增加一次depth的draw call

近处：

1024 \* 1024

![](../../../../.gitbook/assets/image%20%2880%29.png)

512 \* 512

![](../../../../.gitbook/assets/image%20%2876%29.png)

远处：

![](../../../../.gitbook/assets/image%20%2879%29.png)

 **PCF soft:**  接受阴影的对象个数 \* 单像素四次采样,  移动平台：**buit-in PCF\( from gles 3.0\)**，进入静态阴影区以及超出light project camera distance,不需要采样

built-in PCF: 开启纹理比较，加上线性采样器，可以得到pcf的结果

GL\_TEXTURE\_COMPARE\_MODE = GL\_COMPARE\_REF\_TO\_TEXTURE

```text
UNITY_BRANCH
if (shadow > _LightShadowData.r)
	{
		fixed shadowValue = 1.0f;
		float4 ShadowCoord;
		
		//transfer coord
		#ifdef UNITY_NO_SCREENSPACE_SHADOWS
			ShadowCoord = mul(unity_WorldToShadow[0], float4(worldPos, 1.0f));	
		#else
			ShadowCoord = ScreenPos;
		#endif
		//blur
		ShadowCoord.xyz = ShadowCoord.xyz / ShadowCoord.w;
		ShadowCoord.w = 1.0f;

		UNITY_BRANCH
		if (ShadowCoord.z < 0 || (ShadowCoord.z > 1))
			return 1.0;

		#ifdef SHADOWS_SOFT	
			#if defined(UNITY_NO_SCREENSPACE_SHADOWS) && defined(SHADER_API_DESKTOP)
				shadowValue = NssSampleShadowmap_PCF3x3(ShadowCoord, 0);
			#else
				shadowValue = SAMPLE_SHADOW(_ShadowMapTexture, ShadowCoord);//gl3.0 has built-in PCF
			#endif
		#else
			shadowValue = SAMPLE_SHADOW(_ShadowMapTexture, ShadowCoord);
		#endif

		//blend dynamic static shadow
		#ifdef UNITY_NO_SCREENSPACE_SHADOWS
			shadow = min(shadow, _LightShadowData.r + shadowValue * (1 - _LightShadowData.r));
		#else
			shadow = min(shadow, shadowValue);
		#endif

	}
```

#### 未来的考虑：

CSM + LIPSM\(处理较大距离阴影，提高阴影精度）



配置调整：InGameConfig（切换到糖豆车模式）,  car选24210猫车

batches: 126-&gt;121\(关闭阴影），对象类似的灰dynamic batch，所以增加的drawcall都在个位数

![](../../../../.gitbook/assets/image%20%2891%29.png)

### 策划需求:透贴是否可以改为动态阴影

场景需求：阴影距离大于主流游戏，因为我们场景物件少

50 \* 512  

150 \* 512

150 \* 1k 标准配置  

400 \* 2k 

比较shadow2d ,tex2d

一键创建动态阴影。

andreo 506 : 中配 43帧到40帧



如果本体找不到shadowcasterpass ,会向下往fallback里找

"ForceNoShadowCasting" = "True" 可以在shader关闭阴影

通过纹理比较产生的软阴影



方案调研

### 业内其他方案

天涯明月刀：省电，流畅：透贴  精致,高清：主角动态硬阴影（也可能是projector\)

和平精英：流畅，均衡都没有阴影，高清，hdr高清，全人物动态阴影（shadowmap分辨率较低）

codm:流畅，标准，没有阴影，精致，高清，有分辨率较低的阴影。（**吃鸡场景在中配以上有全场景实时阴影**，高配以上有角色阴影）

吃鸡大世界中的大范围阴影是为了保证静态物件的动态阴影可以投射在角色上

**总结： 中低档 没有阴影，高，和超高有阴影。**

### 我们的目标

静态物件烘培阴影不用考虑，

车和人既不投射阴影，也不接收阴影。

动态物件**低配无阴影，高，和超高一定有阴影，中档需要努力优化争取有阴影。**

Unity默认方案对比 200

![&#x5173;&#x95ED;&#x9634;&#x5F71; 96](../../../../.gitbook/assets/image%20%28109%29.png)

![screen space shadow map batches 150](../../../../.gitbook/assets/image%20%28110%29.png)

比较CSSM, shadowProjector, shadowTexture.[**https://www.zhihu.com/question/287079059**](https://www.zhihu.com/question/287079059)**，**CSSM的两级融合。实现方案

**方案**

**单张shadowmap + PCF soft**

阴影bug和难点解决

**相机空间：压缩阴影范围**

shadowCameraSpace 压缩

**相机空间：改进近处阴影精度**

Lightspace shadowMap

[https://www.cnblogs.com/crazii/p/5443534.html](https://www.cnblogs.com/crazii/p/5443534.html)

**自阴影问题**

bias

**软阴影：**

1 PCF

2 ESM 

**静态动态阴影融合问题**

```text
shadow = min(shadowMask, shadowValue);
```

## **优化：**

逻辑地图配置动态物件是否产生阴影

场景地图配置动态物件的阴影

由大圣那边统计动态阴影DC数量。

高配:

| 机型 | iPhone 7P | 8号 | 8号 | 8号 |
| :--- | :--- | :--- | :--- | :--- |
| shadowmap | 阴影关 | 150-1024 | 200-2048 | 400-2048 |
| GPU总+ | 7.28 | 7.87 | 8.58 | 8.59 |
| 主场景 | 4.1 | 4.28 | 4.32 | 4.32 |
| shadowmap总 | - | 0.57 | 1.61 | 1.62 |
| vertex | - | 0.39 | 1.39 | 1.4 |
| fragment | - | 0.28 | 0.3 | 0.3 |
| 总DC | 98 | 114 | 120 | 116 |
| vertices | 201556 | 240504 | 252474 | 244506 |

### **降DC**

**距离优化**：

| 总DC | 98 | 114 | 130 | 150 |
| :--- | :--- | :--- | :--- | :--- |


**没有静态合批，由美术合了，静态走bake shadowmask**

使用标准材质 + 动态物件来限制美术不产生多余的阴影DC，

一键添加动态阴影，一键关掉不必要的投射

**instance batch\(美术材质勾选gpuinstancing,shadowcaster pass兼容，2倍降低DC）**

**dynamic batch\(合批 优化**：美术:限制顶点，确保能动态合批，限制在220\)

过高的DC会导致过高的cpu消耗，限制顶点，确保能够动态合批后

| 总DC | 84 | 88 | 93 | 95 |
| :--- | :--- | :--- | :--- | :--- |


### **降 vs消耗**

**距离减少dc, 同时也减少了vertices，**

**proxy: shadows only receviewshadows off** 

**使用低模proxy投射阴影。包含有多个子材质的，复制一个render出来，改为用一个材质，直接做shadowonly.**

### **降采样范围与shader复杂度：**

**real object:shadows off receviewshadows on**

原始PCF 9次采样，

**使用bilinear sampler + sampler comparision == PCF，仅需一次采样，得到简化但高效的PCF.**

**4 tap PCF得到标准PCF等价结果。**

150 \* 1024

![hard](../../../../.gitbook/assets/image%20%28115%29.png)

![built-in pcf](../../../../.gitbook/assets/image%20%28113%29.png)

![3 \*3 PCF\(4 taps\)](../../../../.gitbook/assets/image%20%28116%29.png)

![screen space shadow map](../../../../.gitbook/assets/image%20%28118%29.png)

![native shadow ma batches 100](../../../../.gitbook/assets/image%20%28114%29.png)



效率数据

**shader If优化**：连片的像素处理能够通过if优化，fragment仅仅跟采样的texture分辨率和采样的范围有关系,

并行的像素处理方式连片的，连片的像素都可以if成功或者失败

| fragment | - | 0.18 | 0.22 | 0.22 |
| :--- | :--- | :--- | :--- | :--- |


| shadowCaster  DC消耗 + shadow collect | 0 | 0.20 | 0.25 | 0.26 |
| :--- | :--- | :--- | :--- | :--- |


超高： 3 \* 3PCF +  400 \* 2048

高配:  built-in PCF + 200 \* 2048

中配 :built-in PCF + 150 \* 1024

与其他方案的对比：

地图5,7，10有问题

场景静态合批被动态阴影打断

Fast Shadow Receiver方案之后，就比较难做融合的效果，除非在新生成的mesh中保存之前mesh的uv2信息以及使用的Lightmap贴图信息，再做一次Lightmap的采样。但这比较麻烦，性价比也不高

{% embed url="https://blog.uwa4d.com/archives/2185.html" %}

dynamic game objects receive low-resolution shadows from static game objects via light probes

0.14ms的固定消耗，

rendershadowmap : 0.20

prepare shadowmap: 0.09 

cullshadowcascades: 0.09 //parellel job to job system

隔帧更新

vs shadow projector, vs CSM ,



