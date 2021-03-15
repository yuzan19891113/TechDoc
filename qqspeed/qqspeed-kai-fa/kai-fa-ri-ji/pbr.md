# PBR

### 直接光

如果不使用PBR,可以使用 简单Blinn-phong模型，

**Diffuse：**Lambert Diffuse = DiffColor \* LightColor \* normal;

**Specular:**  Blinn–Phong specular = SpecColor \*\* LightColor \* pow\(Normal \* \(LightDir + ViewDir\), specularHardness\);

### BRDF



![BRDF&#x65B9;&#x7A0B;](../../../.gitbook/assets/image%20%28142%29.png)

D是Normal Distribution Function\(微表面法线分布\)， G是几何函数，描述的是微平面间相互遮蔽的比率，F是大家耳熟能详的菲涅尔系数，反射与眼睛方向的关系（视角越大，反射越明显\)

half NL = saturate\(dot\(normal, light.dir\)\); 

float NH = saturate\(dot\(normal, halfDir\)\); 

half NV = saturate\(dot\(normal, viewDir\)\); 

float LH = saturate\(dot\(light.dir, halfDir\)\);

**Diffuse**使用Lambert Diffuse = DiffColor \* LightColor \* normal;

**Specular**：

![Cook-Torrance&#x6A21;&#x578B;](../../../.gitbook/assets/image%20%28145%29.png)

* D

一般使用GGX 分布函数，ggx高光拖尾较长

![GGX&#x4E0E;Blinn&#x5DEE;&#x5F02;](../../../.gitbook/assets/image%20%28143%29.png)

移动平台优化版

[https://community.arm.com/developer/tools-software/graphics/b/blog/posts/moving-mobile-graphics](https://community.arm.com/developer/tools-software/graphics/b/blog/posts/moving-mobile-graphics)

![D&#x5206;&#x91CF;\(&#x8003;&#x8651;N,H ,Roughness\)](../../../.gitbook/assets/image%20%28148%29.png)

* G

![G&#x5206;&#x91CF;&#xFF08;&#x8003;&#x8651;L, H, roughness\)](../../../.gitbook/assets/image%20%28151%29.png)

* GF

GF分量合并

![VF&#x5206;&#x91CF;](../../../.gitbook/assets/image%20%28138%29.png)

然后再近似

![](../../../.gitbook/assets/image%20%28146%29.png)

DGF三个分量合并起来的近似

![Optimal mobile PBR\(unity&#x4F1A;&#x5C11;&#x4E00;&#x4E2A;PI&#x5206;&#x91CF;\)](../../../.gitbook/assets/image%20%28149%29.png)

###  间接光

#### Diffuse

* **Static**:LightMap.color \*  DiffColor  \* normal;
* **Dynamic**:sphere harmonics

#### Specular

Reflection cube map: 使用V \* N采样， 根据roughness采样不同mipmaplevel

```text
surfaceReduction * gi.specular* FresnelLerpFast (specColor, grazingTerm, NV);
```



