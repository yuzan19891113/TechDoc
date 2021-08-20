# 代码规范

### 基本规范

#### 1、函数不能过长

函数尽量都写得比较简短，以20-40行左右为佳，功能尽量内聚，函数简短有2个好处：

a.  便于阅读

b.  当外网有bug时，用xlua修改会更加方便

#### 函数复杂度

函数复杂度，指的是函数内分支的数目，分支越多，复杂度越高，复杂度最高不能超过20，超过时，就即合理地提取代码

#### 慎用静态变量和静态类

静态变量因为生命周期贯穿于全游戏，管理不当容易生成内存泄漏，通常不是很底层的功能，静态变量都是可以去除的。容易出事的情况：

a.  静态变量来存储资源（如存GameObject或Mono），然后资源忘记释放了

b.  静态变量当容器，如List, Dictionary，但是忘记清空了，导致内存一直增长

#### List替换成ListView，Dictionary替换成DictionaryView

因为C\#原始的List和Dict里面，会有带有很多不常用的方法，撑大我们的代码段，增加程序的运行时内存。

#### 代码度量

以下三个链接，可以看到我们的代码复杂度，便于定位自己的代码问题

[http://codescene-qqkart.ied.com/login](http://codescene-qqkart.ied.com/login)

[http://v2.devnet.devops.oa.com/console/codecc/speedmobile/task/402993/detail?buildNum=latest](http://v2.devnet.devops.oa.com/console/codecc/speedmobile/task/402993/detail?buildNum=latest)

[http://v2.devnet.devops.oa.com/console/codecc/speedmobile/task/397605/detail?buildNum=latest](http://v2.devnet.devops.oa.com/console/codecc/speedmobile/task/397605/detail?buildNum=latest)

### 进阶规范

#### 接口调用优化

C\#的接口实现和C++的虚函数类似，每次调用时，都会去查表，影响性能。因此，在结果不变的情况下，不要重复调用同一个接口，例子：

```text
// 常用的推荐写法：
ListView<int> iList;
// .Count是List的属性接口，只调用一次，如果写成 i < iList.Count的话，就会重复调用
for(int i = 0, count = iList.Count; i < count; ++i)
    // do something...
```

### Unity代码规范

#### 规避GC，注意返回值是数组的Unity函数

GC容易造成游戏卡顿，如Renderer.materials,  Physics.RaycastAll, AnimationCurve.keys等函数，返回值都是数组，每次调用时都会分配新的数组返回。

随着Unity版本的更新，很多都会有无GC版本的提供，可以由外部传入数组或List，这种情况要改用新版本。

如果没有无GC版本，那么要减少重复调用，如以下这种有问题的写法：

```text
// 以下写法有问题：rd.materials多次访问，每次访问都会产生新的数组分配，产生GC
Renderer rd;
for(int i = 0; i < rd.materials.Length; ++i)
      rd.materials[i].SetFloat(key, value)
```

#### 规避GC，对名字的访问和比较

经常需要在游戏中，比如UnityEngine.Object的名字，如  obj.name == "xxxx",  obj.name.StartsWith\("xxx"\),  obj.name.Contains\("xxx"\)。

每次对obj.name的访问，都会产生分配字符串的GC。常用名字无GC版本的比较函数，在我们引擎中已集成，要使用无GC版本：

```text
UnityEngine.Object obj;
obj.NameEqual("xxx");
obj.NameContains("xxx");
obj.NameStartsWith("xxx");
obj.NameEndsWith("xxx");
```

### 一些反例

TODO

