# Shader

## 标准Shader

![](../../.gitbook/assets/image%20%28177%29.png)



* Combined是单局合批后使用的shader
* CustomFace是捏脸用到的shader（非PBR的，捏脸界面用到的是pbr的）
* GameRules中是一些特殊玩法下的shader，例如支持压扁的
* High是最常用的高模shader
* Low是最常用的低模shader
* Legacy是已经不在使用的一些老旧或者小众shader
* Special是一些特殊需求的小众shader
* Utility是一些程序使用的shader

## PBRshader

![](../../.gitbook/assets/image%20%28178%29.png)



* CustomFace目录下是捏脸界面下超高模角色用到shader
* 其他avatar PBRshader：
  * AvatarStandard 标准shader
  * AvatarStandard\_Low 标准低配shader
  * AvatarStandard\_2Pass 双pass半透标准shader
  * AvatarStandard\_2Pass\_Low 双pass半透标准低配shade

### 支持光照模型

* 标准
* 布料
* 各向异性

### 支持特性

* AA
* 流光

### 光源

* 开启keyword：\_UNITED\_LIGHTING，光源为单局方向光，light prob和 reflection prob
* 关闭则为相机方向光，material中烘焙的SH和material上赋的pre-filtered cubemap 现在**默认keyword为关闭**，这个宏是为将来统一游戏内光照做的准备

## SH烘培工具

![](../../.gitbook/assets/image%20%28182%29.png)

烘培SH到材质球

## Avatar shader规则

老的shader通过不同名称的后缀支持功能,例如：

* \_NoShadow不支持阴影
* \_Spring\_Flatten支持压扁
* ...

比较新的shader例如PBRshader，通过keyword控制，例如：

* SHADOW
* \_AVATAR\_FLATTEN
* ...

材质需求的开发方法与大概流程

