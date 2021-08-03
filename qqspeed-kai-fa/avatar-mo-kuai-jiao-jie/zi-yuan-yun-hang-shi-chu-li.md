# 资源运行时处理

## 加载

### Avatar\(PresentationAvatar\)

#### mPresentationAvatarData:数据组件

mPresentationAvatarDataPart：部件实体

PresentationAvatarDataParam：部件配置参数

### ApplyDressUpInfo

![](../../.gitbook/assets/image%20%28233%29.png)

PresentationAvatarParams.supportPart:部件相互配置隐藏，例如根据某一个头发决定是否隐藏脸

### Pet

Pet:HandleInitSync

### PresentationSuspension

PresentationSuspension:Init

### 人物上车下车处理

Iavatar:SetIsOutOfCar

PresentationAvatarAnim:UpdateShrinkBottom：车内收缩下半身，处理骨骼和可见性

坐姿隐藏mesh：ShouldLoadExpandMesh

![](../../.gitbook/assets/image%20%28198%29.png)

#### 单局内合并DC

低配所有角色单局合并，其他配置只有AI合并,合并要求材质支持

PresentationPartsCombiner.CombineDrawCall:加载需要合并的part，自己，观察的玩家，回放的角色脸不会合并\(需要表情\)

PresentationPartsCombiner.SetupMeshRenderer:设置合并材质

FirePerformanceCombine

combineRender会根据avatarpart是否visible合并mesh

combineMesh-&gt;combineTextures-&gt;compressTexture

套装，宝宝加载每一个Part后会骨架合并，重新蒙皮，单局内还会进一步合并模型和纹理

宠物是一个整体，没有Part

尾挂会在展示宠物后，变身为尾挂

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



