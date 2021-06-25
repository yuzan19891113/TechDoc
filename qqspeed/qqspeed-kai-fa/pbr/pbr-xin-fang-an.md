# PBR新方案

PBR 4档配置

[_https://www.zhihu.com/question/25421190_](https://www.zhihu.com/question/25421190)

[_https://zhuanlan.zhihu.com/p/210221918_](https://zhuanlan.zhihu.com/p/210221918)

## _Motivation_

1. LightMap + shadowMask在一盏主光源的时候合并成一张贴图，Normal + MRA贴图可以合并成NMR + Occlusion贴图，中低配由于不需要Occlusion可以降低一次采样
2. 统一PBR各个档次的PBR模型，shader入口统一为vertstandard, frag\_standard, 模型统一为BRDF\_，通过配置不同的shader feature来控制不同质量等级。
3. ReflectionProbe在中低配的性能与优化（八面体采样\)

## PBR合图

### Normal And MRA 合并为NMR + Occlusion

原本Normal贴图和Metalic ,roughness贴图是分开的

NMR Encoding:

![](https://gblobscdn.gitbook.com/assets%2F-MMLAW2DZhkE5K1kqqvy%2F-MVFWcz7eQimidSk2rgv%2F-MVFgSHjZvgHmV-FRa8w%2Fimage.png?alt=media&token=e9efb846-3c1e-4485-86a1-4df8e41d7065)

l

NMR format:

RGBA ASTC 4X4 能确保法线贴图的准确性

**提高法线精度**

octahedron PackNormal

Decoding:

![](https://gblobscdn.gitbook.com/assets%2F-MMLAW2DZhkE5K1kqqvy%2F-MVFWcz7eQimidSk2rgv%2F-MVFk-AI0Qdwj2yqyFBk%2Fimage.png?alt=media&token=560e1713-f2cd-4ead-afd3-df9e095f95a0)

### Lightmap 合并shadowMask

Encoding: 统一转换为DLDR, 使用默认贴图格式

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-23_14-48-32.png?version=1&modificationDate=1624430913000&api=v2)

Format:

RGBA ASTC 4X4

Decoding:

![](https://gblobscdn.gitbook.com/assets%2F-MMLAW2DZhkE5K1kqqvy%2F-MVFWcz7eQimidSk2rgv%2F-MVFk9d1OXPL8yTJ1vCI%2Fimage.png?alt=media&token=fe181460-a280-4567-aa5c-3cdbca03b6a6)

## 统一PBR

### 低配

**模型：** PBR\_Standard

‌

**Shader Feature:**

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe

**指令数**

|  | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

‌

**真机性能对比：**

|  | **GPU时间** |
| :--- | :--- |
| **PBR\_OPTIMIZE** |  |
| **PBR\_Current** |  |

  
**升级前与升级后效果对比：**

**11城**

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-24_16-23-40.png?version=1&modificationDate=1624523021000&api=v2)

**Alphacity**

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-24_15-30-27.png?version=1&modificationDate=1624519828000&api=v2)

‌

### 中配

‌

**模型**： PBR\_Standard

‌

**Shader Feature:**

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe + **normalMap**

‌

**+ CorrectPlaner\_reflection**

**指令数**

|  Title | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

‌

**真机性能对比：**

|  | **GPU时间** |
| :--- | :--- |
| **PBR\_OPTIMIZE** |  |
| **PBR\_Current** |  |

**效果对比**：

11城

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-25_11-21-54.png?version=1&modificationDate=1624591314000&api=v2)

AlphaCity

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-25_11-22-25.png?version=1&modificationDate=1624591346000&api=v2)

‌

### 高配

‌

模型： PBR\_Standard

‌

**Shader Feature:**

instance + shadowMask + lightmap + **DynamicShadow** + emision + alphaTest + alphaBlend + reflectionProbe + normalMap + CorrectPlaner\_reflection **+ occlusionMap + Direct\_Specular\(大幅增加指令数\)**

**指令数**

|  Title | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

‌

**真机性能对比：**

|  | **GPU时间** |
| :--- | :--- |
| **PBR\_OPTIMIZE** |  |
| **PBR\_Current** |  |

**效果对比：**

**11城**

‌![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-24_16-26-24.png?version=1&modificationDate=1624523184000&api=v2)

**AlphaCity**

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-24_14-54-5.png?version=1&modificationDate=1624517645000&api=v2)

### 超高配

‌

模型： PBR\_Standard

‌

**Shader Feature:**

instance + shadowMask + lightmap + **3 \*3 Soft Shadow** + emision + alphaTest + alphaBlend + normalMap + occlusionMap + Direct\_Specular + CorrectPlaner\_reflection **+ planer\_reflection + DIRLIGHTMAP\_COMBINED**

**指令数**

|  Title | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

‌

**真机性能对比：**

|  | **GPU时间** |
| :--- | :--- |
| **PBR\_OPTIMIZE** |  |
| **PBR\_Current** |  |

**效果对比：**

## ‌优化后的整体效果

![](https://iwiki.woa.com/download/attachments/799924214/image2021-6-23_10-41-16.png?version=1&modificationDate=1624416076000&api=v2)

## 优化后的资源包量与内存对比

场景lightmap + shadowMask →LS  每个场景可以减少1 - 3张贴图

每个模型Normal + MRA →NMR + Occlusion\(选择性生成\) 每个场景可以减少20-40张贴图

## PBR中低配美术流程转换

‌

1 lightmap\(\_final文件\) _+ shadowMask\(\_mask文件\) -&gt; LightMapShadowMask\(_\_LS文件\) （bakery完成后自动合并图，降低贴图数量为1\)\)

‌

2 **Normal贴图** + MRA 贴图 -&gt; NMR\(贴图） + occlusion 贴图\(根据原来MRA中a分量的值， 判断是否生成，就是说绝大部分不需要occlusionMap，降低贴图数量为0.8\),

‌

3 优化PBR模型，暂时统一了pbr模型，使用shaderfeature来区分

‌

4 减少的宏的数量

