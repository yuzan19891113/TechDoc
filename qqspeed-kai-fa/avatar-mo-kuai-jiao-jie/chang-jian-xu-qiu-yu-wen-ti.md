# 常见需求与问题

## 外网与内网常见的错误或者bug原因，包含美术与程序易错点

### 问题1，资源贴图丢失，仅手机端

![](../../.gitbook/assets/image%20%28184%29.png)

#### 原因

AB包问题，因为索引的是公共贴图，套装AB与公共贴图是不同的AB包，因为是增量构建，所以如果套装AB构建时，找不到公共贴图，会导致引用不到资源，等到公共贴图构建的时候，因为是增量构建，原来的套装AB是不会刷新的，导致最终手机包索引不到贴图。

#### 分析过程

使用snapdragon profiler 

1 定位相应DC

2 发现贴图引用不对

3 查看glactivetexture, glbindtexture,确定若干贴图丢失

4 怀疑是AB包数据丢失

5 AB包解包并在PC端加载

#### 解决方法

最简单的是修改套装prefab，触发套装ab构建就行。

### 问题2 Shader变体收集失败问题

Shader 变体没有生效可以通过snapdragon profier分析得到

如果材质运行是换了shader，会导致材质上的keyword对shader收集不成功，因为运行时的这个shader时不知道材质上有这个变体的

### 问题3 Shader兼容问题

使用snapdragonProfiler定位是shader丢失，shader fallback还是兼容问题

如果Shader在手机端有兼容性，如果安卓也有问题，可以通过安卓本地出包解决

### 问题4 大量程序替换shader问题

存在较大的兼容问题，特别是对于shader包含keyword时

## 常见的美术或者程序需求类型

新shader需求

