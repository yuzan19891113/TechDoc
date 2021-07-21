# Avatar动画

## Avatar skin

每个Part包含一个skinmeshrender + sub bone chain

AvatarPart:Reskin:根据sub bone chain的骨骼名字 retarget bones to skeleton bones.

## Avatar Expression

PresentationAvatarExpression:ApplyExpression 加载expressionConfig,确定有多少帧表情，每帧表情的贴图

PresentationAvatarExpression:UpdateExpressioin: 更新时间，选择表情的某一帧



