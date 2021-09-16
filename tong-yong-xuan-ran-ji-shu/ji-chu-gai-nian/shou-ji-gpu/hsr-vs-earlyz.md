# HSR vs earlyZ



#### earlyz

当不透明的图元从Rasterize阶段开始逐像素进行处理时，首先进行Depth Read & Test，通过后直接Write，后续再执行该像素上的PS程序

是需要渲染不透明物体时要按从近到远的顺序去绘制，因为近处的物体先绘制好之后，其写入的深度会阻挡住后面物体的ZTest，也就避免了后面物体无意义的渲染开销

### HSR

iOS TBDR架构的HSR确保遮挡像素肯定不会被draw

earlyz是全局的，是提前绘制好深度，后续所有绘制的dc都会做深度测试

