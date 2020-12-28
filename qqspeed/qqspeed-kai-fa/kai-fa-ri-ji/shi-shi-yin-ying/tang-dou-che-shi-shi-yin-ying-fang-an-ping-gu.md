# 糖豆车实时阴影方案评估

方案：

中高配：动态物件实时阴影+ 静态物件烘培阴影

低配：动态物件无阴影 + 静态物件烘培阴影

低配看时间，如果后期时间有余量，可以考虑优化projector的方法，但优先保证中高配的实时阴影\(参考和平精英，codm中低配都没阴影\)



阴影方案：

单张shadowmap + PCF soft

单张shadowmap: 512\*512\(low\),1024 \* 1024\(medium\),  小于shadow distance（150- 200）的 动态物件增加一次depth的draw call

近处：

1024 \* 1024

![](../../../../.gitbook/assets/image%20%2877%29.png)

512 \* 512

![](../../../../.gitbook/assets/image%20%2875%29.png)

远处：

![](../../../../.gitbook/assets/image%20%2876%29.png)

 PCF soft:  接受阴影的对象个数 \* 单像素四次采样











