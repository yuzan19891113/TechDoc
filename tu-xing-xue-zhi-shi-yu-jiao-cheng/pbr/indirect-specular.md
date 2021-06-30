# Indirect Specular = Environment BRDF

## Equation

![](../../.gitbook/assets/image%20%28211%29.png)

## Prefiltered environment map

蒙特卡洛积分 + GGX重要性采样\(不同粗糙度\)= 不同粗糙度的mipmap level

## Precomputed BRDF \(方程后半部分\)

### LUT

### Simpler Function

1 [https://community.arm.com/developer/tools-software/graphics/b/blog/posts/moving-mobile-graphics](https://community.arm.com/developer/tools-software/graphics/b/blog/posts/moving-mobile-graphics)

![](../../.gitbook/assets/image%20%28208%29.png)

2 [https://www.unrealengine.com/en-US/blog/physically-based-shading-on-mobile](https://www.unrealengine.com/en-US/blog/physically-based-shading-on-mobile)

![](../../.gitbook/assets/image%20%28209%29.png)

如果是没有多少金属性的材质对象

![](../../.gitbook/assets/image%20%28210%29.png)

