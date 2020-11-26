# Shader varient\(shader 变体\)



### 一，multi\_complie 还是 shader\_feature

shader\_feature 和 multi\_complie 是两个很相似的预编译指令，在Editor模式下，他们是几乎没有区别的。

**共同点是：**

* 声明Keyword，用来产生Shader的变体（Variant）

```text
#pragma multi_compile A B
//OR #pragma shader_feature A B

//-----------------------A模块----------------------
#if A
  return fixed4(1,1,1,1); 
#endif 
//---------------------------------------------------

//-----------------------B模块-----------------------
#if B
  return fixed4(0,0,0,1); 
#endif
//---------------------------------------------------
```

* 这个Shader会被编译成两个变体：一是只包含A模块代码的变体A；二是只包含B模块代码的变体B；
* 指定的第一个关键字是默认生效的，即默认使用变体A；
* 在脚本里用Material.EnableKeyword或Shader.EnableKeyword来控制运行时具体使用变体A还是变体B；
* 它们声明的Keyword是全局的，可以对全局的包含该Keyword的不同Shader起作用；
* 全局最多只能声明256个这样的Keyword；
* 请注意Keyword的数量和变体的数量之间的关系，并可能由此导致的性能开销，比如声明\#pragma multi\_compile A B和\#pragma multi\_compile D E 这样的两组Keyword会产生 2x2=4 个Shader变体，但若声明10组这样的keyword，则该Shader会产生1024个变体；

**区别在于：**

**如果使用shader\_feature，build时没有用到的变体会被删除，不会打出来。也就是说，在build以后环境里，运行代码Material.EnableKeyword\("B"\)可能不起作用，因为没有Material在使用变体B，所以变体B没有被build出来，运行时也找不到变体B。**

如果想解决这个问题，可以采取以下办法中的其中一种：

1. 使用multi\_complie 代替 shader\_feature，multi\_complie 会把所有变体build出来；
2. 把这个Shader加入“always included shaders”中 \(Project Settings -&gt; Graphic\)；
3. 创造一个使用变体B的Material，强行说明变体B有用；

### 二，\_\_

上文已经提到了，最多声明256个全局Keyword，因此我们要尽量节省Keyword的使用数量。其中一个技巧是使用 \_\_（两条下划线），如：

```text
#pragma multi_compile __ A
//OR #pragma shader_feature __ A

//-----------------------A模块----------------------
#if A
  return fixed4(1,1,1,1); 
#endif 
//---------------------------------------------------

return fixed4(0,0,0,1);
```

* 此方式相比\#pragma multi\_compile A B 的方式，我们可以减少使用一个Keyword。
* 此方式仍会编译成两个变体：一是不包含A模块代码的变体非A；二是包含A模块代码的变体A；
* 默认为 \_\_ ，即变体非A生效。

### 三，multi\_complie\_local

全局的Keyword只能有256个，这或许会最终对我们造成限制，而且大部分Keyword并不需要进行全局声明。

因此，我们可以使用multi\_complie\_local来声明局部的、只在该Shader内部起作用的Keyword，用法相似：

```text
#pragma multi_compile_local __ A
//OR #pragma shader_feature_local __ A

//-----------------------A模块----------------------
#if A
  return fixed4(1,1,1,1); 
#endif 
//---------------------------------------------------

return fixed4(0,0,0,1);
```

**但需要注意：**

* local Keyword仍有数量限制，每个Shader最多可以包含64个local Keyword
* 因为这种Keyword是局部的，Material.EnableKeyword仍是有效的，但对Shader.EnableKeyword或CommandBuffer.EnableShaderKeyword这种全局开关说拜拜
* 当你既声明了一个全局的Keyword A ，同时又声明了一个同名的、局部的Keyword A，那么优先认为Keyword A是局部的。

