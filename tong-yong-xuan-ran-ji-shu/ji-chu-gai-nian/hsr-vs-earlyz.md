# HSR vs earlyZ

IOS的HSR确保遮挡像素肯定不会被draw，但仅限制在当前写深度的DC,earlyz是全局的，是提前绘制好深度，后续所有绘制的dc都会做深度测试

