# 糖豆车实时阴影方案

### 应用场景

糖豆车若干动态物件的实时阴影。

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





