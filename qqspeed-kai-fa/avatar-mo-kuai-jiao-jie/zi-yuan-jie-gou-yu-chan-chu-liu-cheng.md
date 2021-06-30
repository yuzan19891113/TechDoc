# 资源结构与产出流程

## 资源

包括服装，魔法套装，宠物，宝宝，挂件，表情，人物动作等Avatar相关资源

### 资源的产出流程

teamDeng,运营定义物品ID，资源ID

![Avatar&#x8D44;&#x6E90;&#x5206;&#x7C7B;](../../.gitbook/assets/image%20%28181%29.png)

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

#### 



#### 



### 

