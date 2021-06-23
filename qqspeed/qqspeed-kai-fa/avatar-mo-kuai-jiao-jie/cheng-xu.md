# 程序设计

## 基本模块

### PresentationMgr（渲染管理器\):

AvatarInitConfig Avatar:Init时的avatar质量配置

#### 局外:

UIPlayer:CreatePresentations\_Avatar

* Avatar:ApplyDressUpInfo\(\) ：组装avatar part

PresetnationMgr:OnPresentationEvent\(处理AvatarInit等特效事件\)

#### 局内

Actor:CreateInGameAvatar：玩家，远程玩家会合并DC

Actor:CreateInGameShowAvatar 开场舞玩家

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

## 流程控制

大厅和开场舞都是LobbyAvatarAnimator, 单局是AvatarAnimator

大厅和开场舞会切换不同的animation controller

### 等待房间加载

![&#x52A0;&#x8F7D;&#x5806;&#x6808;](../../../.gitbook/assets/image%20%28190%29.png)

Avatar切换渲染状态，shadow 与noshadow

![](../../../.gitbook/assets/image%20%28192%29.png)

PresentationAvatarExpression: 2D 表情，通过贴图替换实现的

表情是序列帧，每帧由Diffuse和皮肤Mask两张纹理组成， 皮肤Mask的用途是换肤色时防止眼睛变色

![](../../../.gitbook/assets/image%20%28194%29.png)

PresentationAvatar3DExpression: 捏脸

LOD控制: LODMgr.GetAvatarInitConfig AvatarLODController

头发换颜色：

PresentationAvatarMaterialData:TintHair



