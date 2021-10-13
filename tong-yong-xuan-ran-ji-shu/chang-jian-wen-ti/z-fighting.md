# Z Fighting

原因：深度精度问题

方案： 

1 **Physically move the objects further apart**

**2 ** **Increase the near clipping plane and decrease the far clipping plane of the camera**

**3 Depth offset -1 -1**

其他原因

如果nearplane值很小，容易导致误差，例如near0.01使得z_c的高精度部分会集中在0.01附近，而z_c增大的同时，z的误差会越来越大，所以可以通过增大n的方式，来提高近处的深度精度。

一般nearplane应该大于1

![推导设备z的公式，z_c表示camera空间z](<../../.gitbook/assets/image (237).png>)

![f=1000，n=0.01，图中A点表明了Zc∈\[0.01,0.1\]的物体占用了十分之九（0\~0.9）的深度值](<../../.gitbook/assets/image (239).png>)

![n=0.1，f=1000， 前0.9的深度值表示的范围从0.1扩大到了1](<../../.gitbook/assets/image (238).png>)



[https://www.cnblogs.com/jackmaxwell/p/6851728.html](https://www.cnblogs.com/jackmaxwell/p/6851728.html)
