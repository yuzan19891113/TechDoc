# PBR新方案

PBR 4档配置

超高配，高配，中配shader入口统一为vert_standard, frag\_standard,_

_通过配置不同的shader feature来控制不同质量等级。_

低配为vert\_low, frag\_low, 尽量精简算法与shader指令数_。_

### 低配：

模型： PBR\_Standard

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe

**ALU**指令数：

**Sample**次数：

### 中配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe + **normalMap**

**+ CorrectPlaner\_reflection**

**ALU**指令数：

**Sample**次数：

### 高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **DynamicShadow** + emision + alphaTest + alphaBlend  + reflectionProbe + normalMap + CorrectPlaner\_reflection **+ occlusionMap + Direct\_Specular** 

**ALU**指令数：

**Sample**次数：

### 超高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **3 \*3 Soft Shadow**  + emision + alphaTest + alphaBlend + normalMap  + occlusionMap ****+ Direct\_Specular + CorrectPlaner\_reflection **+  planer\_reflection + DIRLIGHTMAP\_COMBINED** 

**ALU**指令数：

**Sample**次数：

### PBR中低配美术流程转换

1 lightmap\(\_final文件\) _+ shadowMask\(\_mask文件\) -&gt; LightMapShadowMask\(_\_LS文件\) （bakery完成后自动合并图，降低贴图数量为1\)\)

2 **Normal贴图** + MRA 贴图 -&gt; NMR\(贴图） + occlusion 贴图\(根据原来MRA中a分量的值， 判断是否生成，理论上较少，降低贴图数量为0.8\),

3 优化PBR模型







