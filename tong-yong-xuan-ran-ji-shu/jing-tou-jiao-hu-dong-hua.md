# 镜头交互动画

### 旋转角色或者物品

Speed\_rotateY += -input.\(mouseButton\).delta.x ;

eulerY += Speed\_rotateY \* deltaTime;

obj.trans.localRotation = Quaternion.Euler\(eulerX, eulerY, eulerZ\);

### 旋转镜头

rotateY  = CurrentRotate.Y;

forEachFrame{

rotateY += input.\(mouseButton\).delta.x ;

TargetRotate = Quaternion.Euler\(CurrentRotate.X, rotateY, CurrentRotate.Z\);

CurrentRotate =Quaternion.slerp\(CurrentRotate , TargetRotate , deltaTime\* damping\)

LookDir = -Vector3.forward \* CurrentRotate \* LookDistance;

Camera.rotate = CurrentRotate;

Camera.Pos = Camera.Target + LookDir

}





