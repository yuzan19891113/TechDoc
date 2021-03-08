# PBR中低配

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



