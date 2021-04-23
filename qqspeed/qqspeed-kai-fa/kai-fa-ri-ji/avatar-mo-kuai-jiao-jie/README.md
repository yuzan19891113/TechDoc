# Avatar模块交接

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



## 疑问

### 外网与内网常见的错误或者bug原因，包含美术与程序易错点，

美术资源问题，材质配置错误问题，新shader兼容性问题

### 常见的美术或者程序需求类型

新shader需求







