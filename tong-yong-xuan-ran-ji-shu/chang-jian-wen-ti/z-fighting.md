# Z Fighting

原因：深度精度问题

方案： 

1 **Physically move the objects further apart**

**2**  **Increase the near clipping plane and decrease the far clipping plane of the camera**

**3 Depth offset -1 -1**

其他原因

如果nearplane值很小，容易导致误差，一般nearplane应该大于1

![](../../.gitbook/assets/image%20%28237%29.png)



