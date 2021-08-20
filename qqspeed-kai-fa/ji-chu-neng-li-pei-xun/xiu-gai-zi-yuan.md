# 修改资源

## 修改资源 <a id="qsm%E5%BC%95%E6%93%8E%E9%80%9A%E7%94%A8%E8%83%BD%E5%8A%9B%E5%9F%B9%E8%AE%AD-%E2%80%94%E2%80%94-%E4%BF%AE%E6%94%B9%E8%B5%84%E6%BA%90"></a>

![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626420345_20.png)

### 总览 <a id="%E6%80%BB%E8%A7%88"></a>

实时修改资源一般是快速定位表现问题的方法，通过替换材质Shader可以确定表现问题是否由材质引起，修改参数和替换贴图则可对Shader的部分Feature进行问题排查，而替换Mesh则可以快速确定是否由Mesh问题引起的表现异常。  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626420011_25.png)

### 关闭对象 <a id="%E5%85%B3%E9%97%AD%E5%AF%B9%E8%B1%A1"></a>

#### 用途 <a id="%E7%94%A8%E9%80%94"></a>

用于在多个半透明重叠物体下快速确定是哪个导致表现异常，以及快速排除灯光、Reflection probe的表现影响。同时也能减少显示对象内容以便于截帧分析、性能测试

#### 编辑器 <a id="%E7%BC%96%E8%BE%91%E5%99%A8"></a>

Hierachy 下选中物体，直接Deactive  
在Scene界面选中物体时，如果物体重叠无法第一次选中，则可多次点击相同位置循环选中

#### 移动端 <a id="%E7%A7%BB%E5%8A%A8%E7%AB%AF"></a>

**Mobile Hierachy**

\[Debug\] &gt; \[MobileHierarchy\]  
![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626406514_53.png)  
可搜索物体、勾选和取消勾选物体

**GM 命令**

打开Mobile Hierachy 会增加额外的CPU开销，在已知物体名称的情况下，可用GM命令，\[Debug\] &gt; \[GM命令\]

* 隐藏物体
* 命令： `clt SetDeactive [物体名称]`
* 范例： `clt SetDeactive Sun`

### 修改材质 <a id="%E4%BF%AE%E6%94%B9%E6%9D%90%E8%B4%A8"></a>

在已知是哪个物体表现异常，可以对材质进行修改来验证材质各项功能。

#### 移动端 <a id="%E7%A7%BB%E5%8A%A8%E7%AB%AF-3"></a>

**GM 命令**

其中材质InstanceID 可以通过Mobile Hierachy 查得

**查询**

* 获取指定材质Shader名：找到指定InstanceID的材质并输出Shader名称，用于查询材质Shader是否正确、是否丢失
* 命令： `clt GetMaterialShader [InstanceID]`
* 范例： `clt GetMaterialShader 123456`
* 获取指定材质的参数：找到指定InstanceID的材质并输出指定参数，用于检查材质参数是否正确，如果是贴图则会输出贴图名称
* 命令： `clt GetMaterialProperty [InstanceID] [float|texture|color] [参数名]`
* 范例： `clt GetMaterialProperty 123456 texture _MainTex`
* 获取全局Shader的参数：调用Shader.GetGlobal系列函数输出指定全局Shader参数，用于检查参数是否正确，如果是贴图则会输出贴图名称
* 命令： `clt GetShaderGlobalProperty [float|texture|color] [参数名]`
* 范例： `clt GetShaderGlobalProperty color _SunColor`
* 获取指定材质的Keyword：找到指定InstanceID的材质并输出Keyword，用于检查材质Keyword是否正确
* 命令： `clt GetMaterialKeyword [InstanceID]`
* 范例： `clt GetMaterialKeyword 123456`

**修改**

* 替换指定所有Shader：找到当前所有用到了该Shader的材质替换为另一个Shader
* 命令： `clt ReplaceAllShader [原Shader名] [目标Shader名]`
* 范例： `clt ReplaceAllShader QF_PBR/Car/Car_Paint_PBR_NFS QF_PBR/Car/Car_Paint_PBR_Low`
* 替换指定材质Shader：当前材质替换为另一个Shader
* 命令： `clt ReplaceMaterialShader [InstanceID] [目标Shader名]`
* 范例： `clt ReplaceMaterialShader 123456 QF_PBR/Car/Car_Paint_PBR_Low`
* 替换并展示贴图：当前材质替换Shader为Unlit/Texture，并将指定参数的贴图作为MainTex赋值，多用于在指定模型上展示某个贴图以确认贴图和模型是否匹配
* 命令： `clt ReplaceMaterialUnlitTexture [InstanceID] [贴图参数]`
* 范例： `clt ReplaceMaterialUnlitTexture 123456 _MaskTex1`

![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626420152_8.png)

* 设置指定材质的参数：找到指定InstanceID的材质并设置指定参数，用于检验材质参数功能是否正确，如果是贴图则需要指定ResLoader加载路径
* 命令： `clt SetMaterialProperty [InstanceID] [float|texture|color] [参数名]`
* 范例： `clt SetMaterialProperty 123456 color _Color 1,1,1,1` `clt SetMaterialProperty 234567 texture _MainTex Vehicles/Car/01NCar/NCar_00001/Textures/NCar_00001_Paint`
* 设置全局Shader的参数：调用Shader.SetGlobal系列函数设置指定全局Shader参数，用于参数功能是否正确，如果是贴图则需要指定ResLoader加载路径
* 命令： `clt SetShaderGlobalProperty [float|texture|color] [参数名]`
* 范例： `clt SetShaderGlobalProperty color _SunColor 1,1,1,1`
* 设置材质的的Keyword：找到指定InstanceID的材质并设置Keyword为Enable或者Disable，用于检查材质Keyword功能是否正确
* 命令： `clt SetMaterialKeyword [InstanceID] [Keyword名] [0为关闭|1为开启]`
* 范例： `clt SetMaterialKeyword 123456 NSS_FOG 0`

### 替换Mesh <a id="%E6%9B%BF%E6%8D%A2mesh"></a>

修改替换模型Mesh，用于排除是否因为模型UV、法线、点色等问题导致的显示异常

**GM 命令**

* 替换模型Mesh为Unity内置几何体类型——Sphere,Capsule,Cylinder,Cube,Plane,Quad
* 命令： `clt ReplacePrimitiveMesh [物体名称] [几何体]`
* 范例： `clt ReplacePrimitiveMesh Car_Lod0_Mesh Sphere` ![undefined](http://tapd.oa.com/tfl/pictures/202107/tapd_10124081_1626419956_44.png)

