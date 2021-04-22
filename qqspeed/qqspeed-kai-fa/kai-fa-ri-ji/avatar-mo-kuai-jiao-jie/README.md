# Avatar模块交接

## 程序

### PresentationMgr（渲染管理器\):

PresentationMgr:CreateAvatar\(创建avatar\)

PresetnationMgr:OnPresentationEvent\(处理AvatarInit等特效事件\)

### Presentation\(渲染对象\):

#### 继承对象

PresentationAvatar, PresentationBabyAvatar, PresentationSkateAvatar

#### 组件

PresentationAvatarWing,

PresentationAvatarHang,

PresentationAvatarAni,

PresentationAvatarBone,

PresentationAvatarExpression

PresentationAvatarDress

PresentationAvatarSkin

等等

### PresentationEventHandler

PresentationAniConfig:主要是特效事件

PresentationAvatarEventHandler

### NssAnimator

LobbyAvatarAnimator

AvatarAnimator, MotorAvatarAnimator,skateAvatarAnimator

## 资源

包括服装，魔法套装，宠物，宝宝，挂件，表情，人物动作等Avatar相关资源

### 资源的产出流程

teamDeng,运营定义物品ID，资源ID

![Avatar&#x8D44;&#x6E90;&#x5206;&#x7C7B;](../../../../.gitbook/assets/image%20%28181%29.png)

### 资源的目录结构与打包更新规范

Artwork

fbx 与materials,textures,common/animation

fbx有高模，低模fbx

#### ResforAssetbundles

prefab, config， 30expression， 00Root， 31Skin

prefab有高模，低模prefab

config配置特效,一般是不带高低模前缀（H,M,L\)的prefab,引用脚本presentation avatar params

30expression,表情是纹理序列帧

00Root，动画Controller, HFCharacterRoot, LFCharacterRoot,骨架

31Skin:ramp图，换肤用的，控制diffuse光照

#### SVN

新套装在主线，旧套装在资源线Ext

#### 资源大小

#### 打包大小

#### 内存大小

### 资源的运行时功能如加载卸载使用

#### 加载

PresentationAvatarDtaPart:LoadAvatarPart

Pet:HandleInitSync

PresentationSuspension:Init

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

### 资源的性能规范与落地机制

#### LOD规范

套装：2 层 

宠物: 2层

尾挂:1层

除了模型面数，节点结构，骨架TPose信息，需要完全一样

#### 使用规则

高模：大厅，UI展示，单局开场舞，单局结算

低模：单局/休闲区

#### 性能

CPU

套装，宠物，尾挂都带动画状态机，影响CPU计算

单局内合批了，DC增加较小

休闲区是没合批的，DC增加较大，休闲区限制了同屏人数

GPU

大厅：材质使用高配，使用了非pbr材质，性能好

单局：合并后使用diffuse图，性能最优

休闲区:简版材质，性能较好

内存

![&#x5185;&#x5B58;&#x7EDF;&#x8BA1;](../../../../.gitbook/assets/image%20%28180%29.png)

### 资源的渲染表现，业内标杆与后续扩展等展望

闪耀暖暖：使用substance标准工作流，多层UV，程序化纹理，PBR, 三维扫描

现状：传统非PBR卡通渲染，2层uv,纹理在photoshop手绘，模型在3dsmax搭建

[https://docs.qq.com/slide/DV1NyRExhaWRzdXJJ](https://docs.qq.com/slide/DV1NyRExhaWRzdXJJ)

## 疑问

外网与内网常见的错误或者bug原因，包含美术与程序易错点

其他常见的美术需求







