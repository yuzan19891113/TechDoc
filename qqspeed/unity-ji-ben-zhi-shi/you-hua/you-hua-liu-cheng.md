# 优化流程

1 设备等级划分， 根据cpu和gpu天梯

2 unity 各种等级设定lod，每个shader需要分不同shader lod值分别撰写shader

3 unity mobile profiler: 

1. adb devices确保连接
2. adb forward tcp:34999 localabstract:Unity-xxxx , xxxx是游戏的包名。

4 性能问题：

1 CPU: 

* GC问题，主要是装箱，listadd等。
* drawcall问题， 是否合批（static batch, dynamic batch由player settings开启\)

2 GPU优化：最简单的通过改分辨率可以快速判断是否gpu瓶颈

1.  overdarw,
2.  shader负载度（填充率，fillrate\)
3.  staticbatch会号内存，dynamic batche会耗cpu（动态顶点buffer\)

3 内存优化

1. 压缩纹理和动画
2. 定期UnLoadUnusedAssets\(GC\)





4 耗电优化

5 中配占最大\(40%以上\)，中配要着重优化

