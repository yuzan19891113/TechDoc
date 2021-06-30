# PBR

知识流程图

1 irradiance vs radiance （单位面积光通量 vs 单位面积单位立体角光通量\)

2 渲染方程（从观察位置,观察角观察到的反射量 =  观察位置自发光 发射量 + 观察位置半球积分入射量），

![](../../.gitbook/assets/image%20%28205%29.png)

3  渲染方程 + 蒙特卡罗积分\(大数定理，大数均值趋近于期望\) + 重要性采样\(降低期望误差\) =  全局光照，光线追踪（PBRT, physical based ray tracing）

4 反射类型， BRDF\(双向反射分布函数\), 折射， 次表面散射BSSRDF

4双向反射分布函数BRDF（反射方程 反射率函数fr，入射，反射能量比例, 描述材质属性\) ,

5 Cook-Torrance BRDF 微面元 D\(法线分布函数NDF\), F\(fresnel效应，Schlick’近似\), G\(几何遮蔽效应\)

![](../../.gitbook/assets/image%20%28200%29.png)

6 预计算BRDF（预过滤环境贴图\) = 蒙特卡洛积分 + GGX重要性采样\(不同粗糙度\)= 不同粗糙度的mipmap level

![&#x524D;&#x534A;&#x90E8;&#x5206;&#x53EF;&#x4EE5;&#x9884;&#x8BA1;&#x7B97;](../../.gitbook/assets/image%20%28204%29.png)

7 最常用BRDF模型 Lambert Diffuse,  L \* N， GGX Specular\(一种特殊的DFG函数,表现高光光晕更好\)

[https://www.cnblogs.com/timlly/p/10631718.html](https://www.cnblogs.com/timlly/p/10631718.html)



