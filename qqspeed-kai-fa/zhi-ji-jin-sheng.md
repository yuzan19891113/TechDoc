# 职级晋升

## 统一PBR升级与优化

问题背景：

车 ，人，场景使用了不同的pbr,不同的光照输入造成了光照和渲染结果的不统一

统一PBR模型

统一不同等级的优化

统一不同输入的pack, normal, metallic, roughness

normal使用octahedron encode normal&#x20;

shadowmask合并贴图，内存，性能，低配上的性能优化，低配走高配效果

## Avatar渲染与物理系统的重新设计与深入优化

渲染：pbr cloth + hair + skin 手机平台的优化，NPR的设计与优化

优化:合并dc绘制，贴图合图与压缩

物理：dynamicbone与cloth，c++ && 并行优化 && 无锁设计



一种无锁的异步simulate dynamicbone方法

simd优化[https://zhuanlan.zhihu.com/p/49188230](https://zhuanlan.zhihu.com/p/49188230)

## 外包美术工具流

发布，同步，清理流水线的设计

一种CP与内网美术无感知的交互方案
