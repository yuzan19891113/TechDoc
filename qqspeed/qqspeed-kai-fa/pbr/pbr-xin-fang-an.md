# PBR新方案



PBR 4档配置

超高配，高配，中配shader入口统一为vert_standard, frag\_standard,_

_通过配置不同的shader feature来控制不同质量等级。_

低配为vert\_low, frag\_low, 尽量精简算法与shader指令数_。_

### 低配：

模型： PBR\_low

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend

### 中配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap + emision + alphaTest + alphaBlend + **normalMap**

### 高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **DynamicShadow** + emision + alphaTest + alphaBlend + normalMap + **occlusionMap**

### 超高配

模型： PBR\_Standard

特性:

instance + shadowMask + lightmap  + **DynamicShadow** + emision + alphaTest + alphaBlend + normalMap + **occlusionMap + planer\_reflection** 





