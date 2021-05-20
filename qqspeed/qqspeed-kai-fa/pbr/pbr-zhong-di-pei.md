# PBR中低配

### PBR不同配置

| 贴图类型 | 高与超高 | 中配 | 低配 |
| :--- | :--- | :--- | :--- |
| BaseColor | Yes | Yes | Yes |
| NormalMap | Yes | **No** | **No** |
| MRA | Yes | Yes | Yes |
| LightMap | Yes | Yes | Yes |
| ShadowMask | Yes | Yes | Yes |
| ReflectionProbe | Yes | Yes | **No** |



| 功能 | 超高配 | 高配 | 中配 | 低配 |
| :--- | :--- | :--- | :--- | :--- |
| BRDF函数 | PBR\_All | PBR\_All | PBR\_All | No |
| 间接 Specular | planer reflection + reflection Cube | reflection Cube | reflection Cube | **No** |
| NormalMap | Yes | Yes | **No** | **No\(shader不支持）** |
| OcclusionMap | Yes | Yes | No | No |

#### 其中高配\_DIRECT\_SPEC\_ON默认开启了，反而超高配可以配置

#### frag\_mid没有使用，理论上应该去掉

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

![](../../../.gitbook/assets/image%20%28133%29.png)

PC: RGBA 32 bit 

mobile: RGBA ASTC 4X4

#### Decoding

![](../../../.gitbook/assets/image%20%28131%29.png)

### LightMap And ShadowMask

gamma与线性的地图的差异处理

* lightmap bake in gamma,  lightMap = 5 \* _data.a  \*_  data.rgb;
* lightmap bake in linear,  lightMap = 5 ^2.2 \*  _pow\(data.a, 2.2\)_  data.rgb

原来的Lightmap ,shadowMask，

PC端Lightmap是RGBM, 移动端是DLDR

现在统一DLDR

#### LS Encoding:

![](../../../.gitbook/assets/image%20%28137%29.png)

PC: RGBA 32 bit 

mobile: RGBA ASTC 4X4

![](../../../.gitbook/assets/image%20%28134%29.png)

#### Decoding

![&#x9AD8;&#x914D;&#x4F18;&#x5316;&#x524D;](../../../.gitbook/assets/image%20%28136%29.png)

![&#x9AD8;&#x914D;&#x4F18;&#x5316;&#x540E;\(&#x5C11;&#x4E00;&#x6B21;&#x91C7;&#x6837;&#xFF0C;&#x6548;&#x679C;&#x4E0D;&#x53D8;\)](../../../.gitbook/assets/image%20%28132%29.png)

![&#x4E2D;&#x914D;PBR&#x4F18;&#x5316;&#x524D;&#xFF08;&#x65E0;normal map&#xFF09;](../../../.gitbook/assets/image%20%28173%29.png)

![&#x4E2D;&#x914D;PBR&#x4F18;&#x5316;&#x540E;\(&#x5C11;&#x4E00;&#x6B21;&#x91C7;&#x6837;&#xFF0C;&#x589E;&#x52A0;&#x4E86;&#x6CD5;&#x7EBF;&#x8D34;&#x56FE;\)](../../../.gitbook/assets/image%20%28168%29.png)

![&#x4F4E;&#x914D;&#x4F18;&#x5316;&#x524D;&#xFF08;&#x65E0;reflection cube&#xFF09;](../../../.gitbook/assets/image%20%28170%29.png)

![&#x4F4E;&#x914D;&#x4F18;&#x5316;&#x540E;\(&#x5C11;&#x4E00;&#x6B21;&#x91C7;&#x6837;&#xFF0C;&#x589E;&#x52A0;&#x4E86;&#x73AF;&#x5883;&#x53CD;&#x5C04;\)](../../../.gitbook/assets/image%20%28169%29.png)

### 测试环境

iphone 8P  新反向11城

![&#x65B0;&#x53CD;&#x5411;11&#x57CE;&#xFF08;Level\_11CityNew\_Reverse\_Art.unity\)](../../../.gitbook/assets/image%20%28155%29.png)

### 测试结果

| 差异 | 超高配 | 高配 | 中配 | 低配 |
| :--- | :--- | :--- | :--- | :--- |
| **原始GPU时间** | 无 | 8.3ms | 6.8ms | 5.8ms |
| PBR采样优化 | 少1次采样 | 少1次采样 | 少1次采样 | 少1次采样 |
| **时间** | 无 | 7.94ms | 6.3ms | 5.5ms |
| PBR效果提升 | 无 | 无 | 增加NormalMap | 增加reflectionProbe采样，不走旧的specular计算 |
| **时间** | 无 | 无 | 6.75ms | 5.4ms |

