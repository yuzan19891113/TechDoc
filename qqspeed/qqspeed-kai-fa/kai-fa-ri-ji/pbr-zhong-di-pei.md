# PBR中低配

### PBR不同配置

| 功能 | 超高配 | 高配 | 中配 | 低配 |
| :--- | :--- | :--- | :--- | :--- |
| BRDF函数 | PBR\_All | PBR\_All | PBR\_All | No |
| 间接 Specular | planer reflection + reflection Cube | reflection Cube | reflection Cube | No |
| NormalMap | Yes | Yes | No | No\(shader不支持） |
| OcclusionMap | Yes | Yes | No | No |

#### 其中高配\_DIRECT\_SPEC\_ON默认开启了，反而超高配可以配置

#### frag\_mid没有使用，理论上应该去掉

低配中的reflection Cube采样未使用，理论上会被shader编译优化掉

#### **结论：**

高配，超高配会降低2次采样，优化了性能， 

中配：降低了1次采样，增加了NormalMap,画质有提升，性能待比较

低配：降低了1次采样，增加reflectonProbe的采样,修改spec计算，性能待比较

#### **测试方法**

什么也不改的优化

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

原来的Lightmap ,shadowMask，

PC端Lightmap是RGBM, 移动端是DLDR

#### LS Encoding:

![](../../../.gitbook/assets/image%20%28137%29.png)

PC: RGBA 32 bit 

mobile: RGBA ASTC 4X4

![](../../../.gitbook/assets/image%20%28134%29.png)

#### Decoding

![&#x5408;&#x5E76;&#x8D34;&#x56FE;&#x524D;](../../../.gitbook/assets/image%20%28136%29.png)

![&#x5408;&#x5E76;&#x8D34;&#x56FE;&#x540E;](../../../.gitbook/assets/image%20%28132%29.png)



