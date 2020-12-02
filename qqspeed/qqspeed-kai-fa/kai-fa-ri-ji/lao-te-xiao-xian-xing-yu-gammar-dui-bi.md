# 老特效线性与Gammar对比

对比颜色空间，从rgb-&gt;hsv-&gt;lab

简化修改hsv的方法

{% embed url="https://zhuanlan.zhihu.com/p/21605186" %}

颜色空间差异对比图



![&#x56FE;&#x7247;1&#xFF1A;Gamma&#x7A7A;&#x95F4;&#x4E0E;Linear&#x7A7A;&#x95F4;](../../../.gitbook/assets/image%20%286%29.png)

Lab空间差异

图片

![Lab&#x7A7A;&#x95F4;&#xFF1A;&#x5DE6;&#x5230;&#x53F3;&#x4F9D;&#x6B21;&#x4E3A;L,a,b&#x5206;&#x91CF;&#x7684;&#x5DEE;&#x5F02;\(b&#x5206;&#x91CF;&#x63A5;&#x8FD1;&#x4E8E;0&#xFF0C;&#x5DEE;&#x5F02;&#x90FD;&#x662F;&#x6574;&#x4F53;&#x5355;&#x5411;&#x7684;\)](../../../.gitbook/assets/image%20%287%29.png)

HSV空间差异

![HSV&#x7A7A;&#x95F4;&#xFF1A;&#x5DE6;&#x81F3;&#x53F3;&#x5206;&#x522B;&#x662F;&#x662F;H,S,V&#x5206;&#x91CF;&#x5DEE;&#x5F02;\(&#x4E09;&#x4E2A;&#x996D;&#x91CF;&#x90FD;&#x6709;&#x5DEE;&#x5F02;&#xFF0C;&#x5E76;&#x4E14;&#x5DEE;&#x5F02;&#x975E;&#x5355;&#x5411;\)](../../../.gitbook/assets/image%20%288%29.png)

刷不出来的特效：移动较快，并且移动本身就有亮度的变化，就会造成单帧差异大，整体看不出来

结果处理方法:取前，上，左的最大，每帧最大





