# Unity使用技巧

* 查看log:C:\Users\YOUR\_USERNAME\AppData\Local\Unity\Editor
* unity更新不起效：编译有error
* unity执行不到对应代码：出现null，导致不能执行到流程
* unity 在非editor代码开发中如果是仅仅editor使用，函数体内部需要加unityeditor宏
* Unity某些script奇怪的变为disable，可以考虑脚本是不是有些函数会变null
* 查找脚本引用的资源的方法，找出脚本metadata里的guid,然后全文件夹搜索
* editui.beginChangecheck, endchangecheck判断属性是否发生了变化，如果是monobehaviour,数据发生变化时会调用onvalidiate\(\)
* 脚本与类同名时才能识别
* 使用Invokerepeating在monobehaviour脚本内部重复调用函数
* texture2d.getpixels需要从gpu回读，所以特别慢
* 修改特效的属性，不能直接particle.main调用，因为只有get权限，可以赋值给var，然后修改
* New出来的Texture2D默认是srgb，就是说采样texture会自动pow2.2，转线性
* 内存不足不会有dump
* 使用bakery烘培时，如果出现阴影烘培不上去，发现shadowmask正常，尝试删掉所有bakerylight,lightmapdata重新生成一下，bakerylight + lightmapdata可能拥有shadowmask uv数据，可能会影响bake shadowmask的结果和对象是否投射动态阴影（根据是否拥有shadowmask uv\)
* 一定要确保shadowmask不变灰，如果变灰，怀疑是bakery或者其他操作把windows lighting 变为subtractive,一定要是shadowmask,一定要确保shadowmask显示正确
* bakery参数与前面不一致一定要警惕，肯定是bakery light不对了，删掉重新烘培
* c\#设置material的时候，从sharematerials的单个material set不行，要对sharematerials整体set
* Batches \(draw calls\) and SetPasses \(material changes\)
* unity default skybox，need a material 产生一个DC。







