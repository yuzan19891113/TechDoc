# PBR中低配

### 

### PBR不同配置

![BRDF&#x65B9;&#x7A0B;](../../../.gitbook/assets/image%20%28139%29.png)

NL表示lightDir \* Normal， D是Normal Distribution Function\(微表面法线分布\)， G是几何函数，描述的是微平面间相互遮蔽的比率，F是大家耳熟能详的菲涅尔系数，反射与眼睛方向的关系（视角越大，反射越明显

#### 超高配：CreateIndirectLight +  BRDF2\_PBS

直接光： Lambert Diffuse \(DiffColor \* LightColor \* NL\)+ 

#### 高配：CreateIndirectLight\_low +  BRDF2\_PBS

#### 中配：CreateIndirectLight\_Low + 



1中低配的效率问题， 采样次数

2 法线压缩策略

3 MRA压缩变为NMR压缩文件后缀为\_NMR

4 Lightmap 合并shadowmask文件后缀为\_LS

5 原本采样次数 4次，变为2次

### Normal an Metalic roughness 

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



