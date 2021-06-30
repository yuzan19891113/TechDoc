# C\#编译

unity2019使用CSC.exe来编译

[https://github.com/dotnet/roslyn.git](https://github.com/dotnet/roslyn.git)

预编译配置：csc.rsp

编译错误查找：

1 确认是否有asmdef,同级目录以及子目录都会编译成asmdef，不存在的上一级去找asmdef,

asmdef里面会指定引用的外部模块

2  导入资源时确保原始资源路径没有改变，如果改变就要很小心，外部资源路径改变并不会使本地资源路径改变。只能删除本地资源再导入





