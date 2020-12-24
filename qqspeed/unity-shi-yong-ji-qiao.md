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
* 使用bakery烘培时，如果出现阴影烘培不上去，发现shadowmask正常，尝试删掉所有bakerylight,lightmapdata重新生成一下







