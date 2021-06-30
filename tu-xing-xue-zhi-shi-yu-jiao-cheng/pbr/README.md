# PBR

知识流程图

1 irradiance vs radiance （单位面积光通量 vs 单位面积单位立体角光通量\)

2 渲染方程（从观察位置,观察角观察到的反射量 =  观察位置自发光 发射量 + 观察位置半球积分入射量），

![](../../.gitbook/assets/image%20%28202%29.png)

3  渲染方程 + 蒙特卡罗积分\(大数定理，大数均值趋近于期望\) + 重要性采样\(降低期望误差\) =  全局光照，光线追踪（PBRT, physical based ray tracing）

4双向反射分布函数BRDF（反射率函数，入射，反射能量比例, 描述材质属性\) ,

5Lambert Diffuse,  L \* N

6 GGX Specular, 微面元 D\(法线分布函数NDF\), F\(fresnel效应，Schlick’近似\), G\(几何遮蔽效应\)

7其他反射分布，折射，次表面散射



