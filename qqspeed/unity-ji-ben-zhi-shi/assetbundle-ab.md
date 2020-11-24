# AssetBundle\(AB\)

AssetBundle（之后简称AB）是Unity提供的资源打包文件，一个AB内部包含多个Object，而Object之间是一个多对多的关系，打包好的AB包之间会保留这种关系，那Unity是如何保证AB之间形成正确的引用呢

解码AB包：通过

`C:\Work\unity2017_win\Data\Tools\WebExtract.exe prefab`

解开prefab， 然后生成一个prefab\_data\CAB-8bc5a956f7efa6356fcd1d00c8005f99 序列化文件。

然后通过

`C:\Work\unity2017_win\Data\Tools\binary2text.exe .\prefab_data\CAB-8bc5a956f7efa6356fcd1d00c8005f99`

转化为txt文件查看。

\`\`

