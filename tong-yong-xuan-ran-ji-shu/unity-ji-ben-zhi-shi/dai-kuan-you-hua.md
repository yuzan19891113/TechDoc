# 带宽优化

## 硬件

Memory Bandwidth = Memory speed \* Memory Interface Width / 8

[https://en.wikipedia.org/wiki/List_of_Qualcomm_Snapdragon_processors#Snapdragon\_888\_5G\_(2021](https://en.wikipedia.org/wiki/List_of_Qualcomm_Snapdragon_processors#Snapdragon\_888\_5G_\(2021)

[https://en.wikipedia.org/wiki/List_of_Qualcomm_Snapdragon_processors#Snapdragon\_888\_5G\_(2021](https://en.wikipedia.org/wiki/List_of_Qualcomm_Snapdragon_processors#Snapdragon\_888\_5G_\(2021)) [https://en.wikipedia.org/wiki/LPDDR](https://en.wikipedia.org/wiki/LPDDR)

![各平台带宽数据](<../../.gitbook/assets/image (225).png>)

## 带宽影响

* 延迟
* 耗电
* 其他相关问题

## 带宽原因

### Texture

* Texture尺寸与存储(压缩格式)
* 采样方式(Filter, UV坐标）

### FrameBuffer

* FrameBuffer尺寸与格式
* load/store触发:SetRenderTarget、copy、grabTexture、blit等

{% hint style="info" %}
**Load**: memory to on-chip memory, **store**: on-chip memory to memory
{% endhint %}

### Primitive 数量 or ConstBuffer

## 优化方案

### Texture

在表现达到要求前提：

* 尺寸越小越好 
* 格式通道越少越好 
* 使用压缩格式（离线、实时） 
* mipmap尽量开启（非UI） 
* 采样过滤模式越简单越好 
* 采样时UV坐标越接近越好(采样相近纹理，可以复用texture cache,不用去内存中取)

### FrameBuffer

* 减小尺寸(降低分辨率) buffer: color, depth, stencil color格式：RBGA8, R11G11B10, FP16等 depth/stencil 格式: depth 16, depth 24 stencil 8等
* load, store (mobile) memoryless dontCare, clear

## 实现

### 图形API底层

![Load store in different API](<../../.gitbook/assets/image (218).png>)

loadOp:dontCare, load, clear 

storeOp: dontCare, store, multisampleResolve …

### Unity实现

#### Unity提供的设置load, store方法 

* CommandBuffer.SetRenderTarget(…) 
* Graphics.SetRenderTarget( RenderTargetSetup setup) 
* RenderTexture.memorylessMode
* RenderTextureDescriptor. memorylessMode 
* Clear()

#### RenderTextureMemoryless

* None, 
* Color, 
* Depth, 
* MSAA on-tile memory, 
* redueces memory usage 

note: can’t read or write to these render textures

## 工具

## 案例
