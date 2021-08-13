# Z Fighting

原因：深度精度问题

方案： 

1 **Physically move the objects further apart**

**2**  **Increase the near clipping plane and decrease the far clipping plane of the camera**

**3 Depth offset -1 -1**

其他原因

如果nearplane值很小，容易导致误差，例如near0.01使得z\_c的高精度部分会集中在0.01附近，而z\_c增大的同时，z的误差会越来越大，所以可以通过增大n的方式，来提高近处的深度精度。

一般nearplane应该大于1

![&#x63A8;&#x5BFC;&#x8BBE;&#x5907;z&#x7684;&#x516C;&#x5F0F;&#xFF0C;z\_c&#x8868;&#x793A;camera&#x7A7A;&#x95F4;z](../../.gitbook/assets/image%20%28237%29.png)

![f=1000&#xFF0C;n=0.01&#xFF0C;&#x56FE;&#x4E2D;A&#x70B9;&#x8868;&#x660E;&#x4E86;Zc&#x2208;\[0.01,0.1\]&#x7684;&#x7269;&#x4F53;&#x5360;&#x7528;&#x4E86;&#x5341;&#x5206;&#x4E4B;&#x4E5D;&#xFF08;0~0.9&#xFF09;&#x7684;&#x6DF1;&#x5EA6;&#x503C;](../../.gitbook/assets/image%20%28239%29.png)

![n=0.1&#xFF0C;f=1000&#xFF0C; &#x524D;0.9&#x7684;&#x6DF1;&#x5EA6;&#x503C;&#x8868;&#x793A;&#x7684;&#x8303;&#x56F4;&#x4ECE;0.1&#x6269;&#x5927;&#x5230;&#x4E86;1](../../.gitbook/assets/image%20%28238%29.png)



[https://www.cnblogs.com/jackmaxwell/p/6851728.html](https://www.cnblogs.com/jackmaxwell/p/6851728.html)

