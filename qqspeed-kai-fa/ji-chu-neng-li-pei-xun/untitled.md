# Untitled

## 准备debuggbale的apk <a id="1-%E5%87%86%E5%A4%87debuggbale%E7%9A%84apk"></a>

飞车构建出来的包，默认不带debuggble，需要通过repack来使其变得可调试

### 1.1下载未签名的包 <a id="11%E4%B8%8B%E8%BD%BD%E6%9C%AA%E7%AD%BE%E5%90%8D%E7%9A%84%E5%8C%85"></a>

记得下载非签名包，用签名后的包repack会崩溃  


![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625818698534.png?version=1&modificationDate=1625818698543&api=v2)

### 1.2 repack <a id="12-repack"></a>

工具目录Tools\Debug\RepackDebuggable\repack\_debuggable.py  
需要有java、python环境  
执行python repack\_debuggable.py -i xxx.apk  
  
红框是repack后的apk，这里放置libunity.so也会替换进去repack  


![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625818619719.png?version=1&modificationDate=1625818619748&api=v2)

![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625818406681.png?version=1&modificationDate=1625818406705&api=v2)

## 2. 用AndroidStudio调试 <a id="2-%E7%94%A8androidstudio%E8%B0%83%E8%AF%95"></a>

### 2.1 打开apk <a id="21-%E6%89%93%E5%BC%80apk"></a>

左上角File-&gt;Profile or Debug APK  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625818994502.png?version=1&modificationDate=1625818994563&api=v2)  
选中刚刚repack后的包  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819010626.png?version=1&modificationDate=1625819010646&api=v2)

### 2.2 设置符号表 <a id="22-%E8%AE%BE%E7%BD%AE%E7%AC%A6%E5%8F%B7%E8%A1%A8"></a>

左侧找到libunity.so  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819104792.png?version=1&modificationDate=1625819104801&api=v2)  
右边视图点add  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819129436.png?version=1&modificationDate=1625819129442&api=v2)  
选择匹配的带符号so，要跟repack进包里的so对应上  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819199107.png?version=1&modificationDate=1625819199120&api=v2)  
成功后会自动关联源码  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819625993.png?version=1&modificationDate=1625819626003&api=v2)

### 2.3 调试 <a id="23-%E8%B0%83%E8%AF%95"></a>

右上角attach，也可以直接调试启动  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819282034.png?version=1&modificationDate=1625819282060&api=v2)  
选中游戏进程，非debuggble的包这里识别不到  
![enter image description here](https://iwiki.woa.com/download/attachments/858657766/image-1625819293199.png?version=1&modificationDate=1625819293212&api=v2)  
下图是在Player.cpp主循环中断下来的效果  


