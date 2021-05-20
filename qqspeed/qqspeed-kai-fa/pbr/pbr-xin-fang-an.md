# PBR新方案



PBR 4档配置

超高配，高配，中配shader入口统一为vert_standard, frag\_standard,_

_通过配置不同的shader feature来控制不同质量等级。_

低配为vert\_low, frag\_low, 尽量精简算法与shader指令数_。_

### 低配：

模型： PBR\_low

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + reflectionProbe

### 中配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + **** + reflectionProbe + **normalMap**

### 高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **DynamicShadow** + emision + alphaTest + alphaBlend  + reflectionProbe + normalMap + **occlusionMap**

### 超高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **3 \*3 Soft Shadow**  + emision + alphaTest + alphaBlend + normalMap + **occlusionMap + planer\_reflection** 

\*\*\*\*

### PBR中低配美术流程转换

1 lightmap\(\_final文件\) _+ shadowMask\(\_mask文件\) -&gt; LightMapShadowMask\(_\_LS文件\) （bakery完成后自动合并图\)

2 **Normal贴图** + MRA 贴图 -&gt; NMR\(贴图） + occlusion 贴图 \( 是否程序合并图，怎么集成到美术流程\)

3 优化PBR模型







