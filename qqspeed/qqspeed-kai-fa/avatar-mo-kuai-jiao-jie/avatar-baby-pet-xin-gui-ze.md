# Avatar & baby & pet新规则

### publish线迁移回主线的资源 + unity 2017 老外包编辑器的资源：

第1步:  **程序**：新shader 必须为hidden 兼容gamma的版本，由程序确保，通知美术迁移资源

第2步:  **美术**：迁移美术资源 ，确保贴图为srgb不勾选，美术对比差异，通知程序迁移完成，**是否有差异，**

美术需要后续调整效果，不能换shader，

第3步: **程序**：查看美术材质是否有修改颜色，否则材质需要工具处理\(Linear-&gt;线性工程转换-&gt;材质color转换

![](../../../.gitbook/assets/image%20%28197%29.png)

### 主线新制作资源 + unity 2019 外包编辑器的资源（下个迭代必须使用unity2019外包编辑器制作资源）

1 shader 是非hidden的版本，程序保证

2 非颜色类贴图Mask, _Mxx, 不勾选srgb,  颜色类贴图勾srgb_

_3  common textures 处理_

_4 贴图被pbr 和 hidden共同使用_









