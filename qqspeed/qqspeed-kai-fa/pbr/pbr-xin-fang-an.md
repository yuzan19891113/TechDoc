# PBR新方案

PBR 4档配置

shader入口统一为vert_standard, frag\_standard, 模型统一为BRDF\__

_通过配置不同的shader feature来控制不同质量等级。_

\_\_[_https://www.zhihu.com/question/25421190_](https://www.zhihu.com/question/25421190)\_\_

### 低配：

模型： PBR\_Standard

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe

|  | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

#### 真机对比



### 中配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe + **normalMap**

**+ CorrectPlaner\_reflection**

|  | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

#### 真机对比

### 高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **DynamicShadow** + emision + alphaTest + alphaBlend  + reflectionProbe + normalMap + CorrectPlaner\_reflection **+ occlusionMap + Direct\_Specular\(大幅增加指令数\)** 

|  | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

#### 真机对比

### 超高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **3 \*3 Soft Shadow**  + emision + alphaTest + alphaBlend + normalMap  + occlusionMap ****+ Direct\_Specular + CorrectPlaner\_reflection **+  planer\_reflection + DIRLIGHTMAP\_COMBINED**

|  | **ALU**指令数： | **Register**数量： | **Sample**次数： |
| :--- | :--- | :--- | :--- |
| **PBR\_OPTIMIZE** |  |  |  |
| **PBR\_Current** |  |  |  |

#### 真机对比

### PBR中低配美术流程转换

1 lightmap\(\_final文件\) _+ shadowMask\(\_mask文件\) -&gt; LightMapShadowMask\(_\_LS文件\) （bakery完成后自动合并图，降低贴图数量为1\)\)

2 **Normal贴图** + MRA 贴图 -&gt; NMR\(贴图） + occlusion 贴图\(根据原来MRA中a分量的值， 判断是否生成，就是说绝大部分不需要occlusionMap，降低贴图数量为0.8\),

3 优化PBR模型

4 减少的宏的数量







