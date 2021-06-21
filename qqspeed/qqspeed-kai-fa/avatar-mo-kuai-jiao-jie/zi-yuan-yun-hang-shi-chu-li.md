# 资源运行时处理

#### 加载

PresentationAvatarDataPart:LoadAvatarPart

Pet:HandleInitSync

PresentationSuspension:Init

套装单局坐姿隐藏：ShouldLoadExpandMesh

套装，宝宝加载每一个Part后会骨架合并，重新蒙皮，单局内还会进一步合并模型和纹理

宠物是一个整体，没有Part

尾挂会在展示宠物后，变身为尾挂

人物车内车外处理：

setIsOutOfCar,  

UpdateShrinkBottom\(PresentationAvatar avatar\)\(车内收缩下半身\)

单局内合并DC

FirePerformanceCombine

combineRender会根据avatarpart是否visible合并mesh

#### 卸载

PresentationAvatarDataParts:ClearParts

Pet:Release

PresentationSuspension:Release

### 动态下载

Avatar,宝宝，先装备默认装，等待下载完成再替换

#### 资源报错

Avatar,宝宝，检查骨头，检查TPose,资源不存在\(加载默认\)

宠物，检查配置，资源不存在\(报错\)

尾挂，检查资源不存在\(报错\)

#### 大厅魔法套装

会替换动画

如果材质运行是换了shader，会导致材质上的keyword对shader收集不成功，因为运行时的这个shader时不知道材质上有这个变体的

