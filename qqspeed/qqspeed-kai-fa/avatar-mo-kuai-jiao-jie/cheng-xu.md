# 程序设计

## 基本模块

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

## 流程控制

大厅和开场舞都是LobbyAvatarAnimator, 单局是AvatarAnimator

大厅和开场舞会切换不同的animation controller

### 等待房间加载

![&#x52A0;&#x8F7D;&#x5806;&#x6808;](../../../.gitbook/assets/image%20%28190%29.png)

Avatar回到大厅切换渲染状态，

![](../../../.gitbook/assets/image%20%28191%29.png)

