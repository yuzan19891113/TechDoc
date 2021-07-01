# Unity 2019 c\# call native engine c++

Unity 2019 中引擎的 C++ 函数导出需要修改 xxx.bindings.h 和 xxx.bindings.cs 两个文件：

1. xxx.bindings.h 的作用是将C++方法封装成一个C方法，将对象本身传入其中\(使用C++编辑器编译\)；
2. xxx.bindings.cs 的作用是对C方法的封装\(使用C\#编译器编译\)。

## 增加C\#导出函数方法 <a id="toc0"></a>

### 对静态函数增加接口导出

1 直接使用上面的 FreeFunction 在 xxx.bindings.cs 中标注  
例如要对 Application 增加一个 SetAndroidExtraSearchPath 方法

![enter image description here](http://tapd.oa.com/tfl/pictures/202104/tapd_10124081_1618478522_15.png)

![enter image description here](http://tapd.oa.com/tfl/pictures/202104/tapd_10124081_1618478496_96.png)

![DVM:SetExtraSoSearchPath&#x7684;&#x5BFC;&#x51FA;](../../.gitbook/assets/image%20%28214%29.png)



### 对class成员函数增加导出

 UnityEngine.Object 增加一个 GetNameLen方法：

![&#x589E;&#x52A0;&#x4E00;&#x4E2A;&#x7C7B;&#x7684;&#x65B9;&#x6CD5;](../../.gitbook/assets/image%20%28208%29.png)

在 xxx.bindings.h 中增加函数，函数参数中增加一个对象参数作为第一个参数（按照C预言的语法写即可）

![](../../.gitbook/assets/image%20%28211%29.png)

![enter image description here](http://tapd.oa.com/tfl/pictures/202104/tapd_10124081_1618478348_75.png)

再在 xxx.bindings.cs 中增加 FreeFunction 增加对 xxx.bindings.h 中函数的声明，再实现导出函数，其中对象参数使用 this 传递过来即可。

![enter image description here](http://tapd.oa.com/tfl/pictures/202104/tapd_10124081_1618478331_14.png)

![](../../.gitbook/assets/image%20%28210%29.png)

### C函数声明FreeFunction使用方法： <a id="toc1"></a>

![enter image description here](http://tapd.oa.com/tfl/pictures/202104/tapd_10124081_1618476867_22.png)

理解：

1. FreeFunction 的标准用法是：\[FreeFunction\("SomeNamespace::SomeFreeFunction"\)\];
2. FreeFunction 需要配置NativeHeader 使用，NativeHeader 标注的是函数声明的头文件位置；
3. 如果没有命名空间，可以省略 "SomeNamespace::SomeFreeFunction”， 直接使用 FreeFunction。

