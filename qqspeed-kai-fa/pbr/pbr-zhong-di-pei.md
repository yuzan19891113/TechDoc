# PBR中低配

PBR不同配置

| 贴图类型            | 高与超高 | 中配     | 低配     |
| --------------- | ---- | ------ | ------ |
| BaseColor       | Yes  | Yes    | Yes    |
| NormalMap       | Yes  | **No** | **No** |
| MRA             | Yes  | Yes    | Yes    |
| LightMap        | Yes  | Yes    | Yes    |
| ShadowMask      | Yes  | Yes    | Yes    |
| ReflectionProbe | Yes  | Yes    | **No** |



| 功能           | 超高配                                 | 高配              | 中配              | 低配                |
| ------------ | ----------------------------------- | --------------- | --------------- | ----------------- |
| BRDF函数       | PBR_All                             | PBR_All         | PBR_All         | No                |
| 间接 Specular  | planer reflection + reflection Cube | reflection Cube | reflection Cube | **No**            |
| NormalMap    | Yes                                 | Yes             | **No**          | **No(shader不支持）** |
| OcclusionMap | Yes                                 | Yes             | No              | No                |

#### 其中高配\_DIRECT_SPEC_ON默认开启了，反而超高配可以配置

#### frag_mid没有使用，理论上应该去掉

低配中的reflection Cube采样未使用，理论上会被shader编译优化掉

#### **结论：**

高配，超高配会降低1次采样，occlusionmap是单独的图，优化了性能， 

中配：降低了1次采样，增加了NormalMap,画质有提升，性能待比较

低配：降低了1次采样，增加reflectonProbe的采样,修改spec计算，性能待比较

#### **测试方法**

中配增加NormalMap,画质，性能对比，画质对比

低配增加ReflectionProbe的画质性能对比

#### PBR合图策略

1中低配的效率问题， 采样次数

2 法线压缩策略

3 MRA压缩变为NMR压缩文件后缀为\_NMR

4 Lightmap 合并shadowmask文件后缀为\_LS

5 原本采样次数 4次，变为2次

### Normal And Metalic roughness 

原本Normal贴图和Metalic ,roughness贴图是分开的

#### NMR Encoding:

![](<../../.gitbook/assets/image (133).png>)

PC: RGBA 32 bit 

mobile: RGBA ASTC 4X4

#### Decoding

![](<../../.gitbook/assets/image (131).png>)

### LightMap And ShadowMask

[https://docs.unity3d.com/Manual/Lightmaps-TechnicalInformation.html](https://docs.unity3d.com/Manual/Lightmaps-TechnicalInformation.html)

gamma与线性的地图的差异处理

* lightmap bake in gamma,  lightMap = 5 \* _data.a  \* _ data.rgb;
* lightmap bake in linear,  lightMap = 5 ^2.2 \* _ pow(data.a, 2.2) _ data.rgb

原来的Lightmap ,shadowMask，

PC端Lightmap是RGBM, 移动端是DLDR

现在统一DLDR, max value 是34.49f.

不同hdr value的结果

#### LS Encoding:

![](<../../.gitbook/assets/image (137).png>)

PC: RGBA 32 bit 

mobile: RGBA ASTC 4X4

![](<../../.gitbook/assets/image (134).png>)

#### Decoding

![高配优化前](<../../.gitbook/assets/image (136).png>)

![高配优化后(少一次采样，效果不变)](<../../.gitbook/assets/image (132).png>)

![中配PBR优化前（无normal map）](<../../.gitbook/assets/image (173).png>)

![中配PBR优化后(少一次采样，增加了法线贴图)](<../../.gitbook/assets/image (168).png>)

![低配优化前（无reflection cube）](<../../.gitbook/assets/image (170).png>)

![低配优化后(少一次采样，增加了环境反射)](<../../.gitbook/assets/image (169).png>)

### 测试环境

iphone 8P  新反向11城

![新反向11城（Level\_11CityNew_Reverse_Art.unity)](<../../.gitbook/assets/image (155).png>)

### 测试结果

| 差异          | 超高配   | 高配     | 中配          | 低配                                 |
| ----------- | ----- | ------ | ----------- | ---------------------------------- |
| **原始GPU时间** | 无     | 8.3ms  | 6.8ms       | 5.8ms                              |
| PBR采样优化     | 少1次采样 | 少1次采样  | 少1次采样       | 少1次采样                              |
| **时间**      | 无     | 7.94ms | 6.3ms       | 5.5ms                              |
| PBR效果提升     | 无     | 无      | 增加NormalMap | 增加reflectionProbe采样，不走旧的specular计算 |
| **时间**      | 无     | 无      | 6.75ms      | 5.4ms                              |
