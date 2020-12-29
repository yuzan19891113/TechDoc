# 糖豆车实时阴影方案评估

方案：

中高配：动态物件实时阴影+ 静态物件烘培阴影

低配：动态物件无阴影 + 静态物件烘培阴影

低配看时间，如果后期时间有余量，可以考虑优化projector的方法，但优先保证中高配的实时阴影\(参考和平精英，codm中低配都没阴影\)

阴影方案：

**单张shadowmap + PCF soft**

**单张shadowmap:** 512\*512\(low\),1024 \* 1024\(medium\),  小于shadow distance（150- 200）的 动态物件增加一次depth的draw call

近处：

1024 \* 1024

![](../../../../.gitbook/assets/image%20%2877%29.png)

512 \* 512

![](../../../../.gitbook/assets/image%20%2875%29.png)

远处：

![](../../../../.gitbook/assets/image%20%2876%29.png)

 **PCF soft:**  接受阴影的对象个数 \* 单像素四次采样,PC平台：手写 PCF过滤  移动平台：buit-in PCF\( from gles 3.0\)，进入静态阴影区以及超出light project camera distance,不需要采样

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











