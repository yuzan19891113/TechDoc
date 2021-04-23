# 程序设计

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

