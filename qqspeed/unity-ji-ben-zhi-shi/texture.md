# Texture

HDR Encode:**RGBM** encoding packs \[0;8\] range into \[0;1\] with multiplier stored in the alpha channel. Final value is RGB_A_8.

通过重载SetTexture来控制每一个种类的texture的size，format，防止美术的过度操作。

### ASTC texture compression

ASTC texture compression is an official extension to the OpenGL and OpenGL ES graphics APIs. ASTC can reduce the memory required by your application and reduce the memory bandwidth required by the GPU.

ASTC offers texture compression with high quality, low bitrate and has many control options. It includes the following features:

* Bit rates range from 8 bits per pixel \(bpp\) to less than 1bpp. This enables you to fine-tune the trade-off of space against quality.
* Support for 1 to 4 color channels.
* Support for both low dynamic range \(LDR\) and high dynamic range \(HDR\) images.
* Support for 2D and 3D images.
* Support for selecting different combinations of features.
* The larger block sizes provide higher compression



**if your device supports ASTC, use it to compress the textures in your 3D content. If your device does not support ASTC, try using ETC2.**

**You must differentiate between textures used in 3D content from textures used in the GUI elements. In some cases it might be best to leave the GUI textures uncompressed to avoid unwanted artifacts.**

## **SRGB texture**

在linear space下，写入时会linear-&gt;gammar, 读取时会gammar-&gt;linear

所以如果作为rt使用时没有区别的

