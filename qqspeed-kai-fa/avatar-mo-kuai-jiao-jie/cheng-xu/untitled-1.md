# Avatar动画

## Avatar skin

每个Part包含一个skinmeshrender + sub bone chain

AvatarPart:Reskin:根据sub bone chain的骨骼名字 retarget bones to skeleton bones.

## Avatar Expression

PresentationAvatarExpression:PlayAnimWithExpression 动作的匹配表情\(例如结算表情\)，配置在新物品表

![](../../../.gitbook/assets/image%20%28234%29.png)

![](../../../.gitbook/assets/image%20%28235%29.png)

PresentationAvatarExpression:ApplyExpression 加载expressionConfig,确定有多少帧表情，每帧表情的贴图

PresentationAvatarExpression:UpdateExpressioin: 更新时间，选择表情的某一帧

presentation common params 保存动画与表情的配置

## AvatarPart Animation

mPresentationAvatarAnim.PlayAvatarPartAnim

mPresentaitonAvatarBone.mRootAnimation

