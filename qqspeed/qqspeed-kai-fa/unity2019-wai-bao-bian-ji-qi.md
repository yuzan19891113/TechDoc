# Unity2019 外包编辑器

## 维护流程

### 更新外包引擎

[http://tc-svn.tencent.com/qqspeed/nssunity5\_proj/branches/Unity2019.4.22f1\_externalEditor](http://tc-svn.tencent.com/qqspeed/nssunity5_proj/branches/Unity2019.4.22f1_externalEditor)

取最新引擎，然后合并以上分支的

![](../../.gitbook/assets/image%20%28193%29.png)

编译引擎。

加密工具，内部已更新到csc的解密程序，一般不需要重新生成

[http://tc-svn.tencent.com/qqspeed/nssunity5\_proj/trunk/Libs/mono\_5.0.1.1\_for\_mcs](http://tc-svn.tencent.com/qqspeed/nssunity5_proj/trunk/Libs/mono_5.0.1.1_for_mcs)

编译后需要拷贝roslyn\artifacts\bin\csc\Release的所有文件拷贝覆盖到引擎的WindowsEditor\Data\Tools\Roslyn，从而让引擎具有了编译加密代码文件的功能

### 导出加密基础资源

### 导出补丁

### 通用导出资源

## 加密

### 代码

### Shader

### 资源

## 解密

### 代码

UnityEditor解密只用于展示，真正用于编译的是CSC.exe,将C\#编译成dll\(unity 2017是msc.exe\)

unity用Roslyn, [https://github.com/Unity-Technologies/roslyn.git](https://github.com/Unity-Technologies/roslyn.git),需要安装unity2019

1. 执行Restore.cmd
2.  执行build.cmd

如果需要编译sln，需要执行Restore.cmd后使用unity2019打开Compilers.sln编译,配置编译选项为csc

如果有编译问题，可以查询[https://github.com/dotnet/roslyn/blob/main/docs/contributing/Building%2C%20Debugging%2C%20and%20Testing%20on%20Windows.md](https://github.com/dotnet/roslyn/blob/main/docs/contributing/Building%2C%20Debugging%2C%20and%20Testing%20on%20Windows.md)

编译器需要增加解密已加密的代码片段，修改CommonCompiler.cs,

TryReadFileContent为所有编译文件的读取入口

```text
internal SourceText? TryReadFileContent(CommandLineSourceFile file, IList<DiagnosticInfo> diagnostics, out string? normalizedFilePath)
        {
            var filePath = file.Path;
            try
            {
                if (file.IsInputRedirected)
                {
                    using var data = Console.OpenStandardInput();
                    normalizedFilePath = filePath;
                    using (var reader = new StreamReader(data))
                    {
                        var text = reader.ReadToEnd();
                        text = XXTEA.TryDecrypt(text);
                        return SourceText.From(text, reader.CurrentEncoding);
                    }
                    return EncodedStringText.Create(data, _fallbackEncoding, Arguments.Encoding, Arguments.ChecksumAlgorithm, canBeEmbedded: EmbeddedSourcePaths.Contains(file.Path));
                }
                else
                {
                    using var data = OpenFileForReadWithSmallBufferOptimization(filePath);
                    normalizedFilePath = data.Name;
                    using (var reader = new StreamReader(data))
                    {
                        var text = reader.ReadToEnd();
                        text = XXTEA.TryDecrypt(text);
                        return SourceText.From(text, reader.CurrentEncoding);
                    }

                    return EncodedStringText.Create(data, _fallbackEncoding, Arguments.Encoding, Arguments.ChecksumAlgorithm, canBeEmbedded: EmbeddedSourcePaths.Contains(file.Path));
                }
            }
            catch (Exception e)
            {
                diagnostics.Add(ToFileReadDiagnostics(this.MessageProvider, e, filePath));
                normalizedFilePath = null;
                return null;
            }
        }
```

编译后需要拷贝roslyn\artifacts\bin\csc\Release的所有文件拷贝覆盖到引擎的WindowsEditor\Data\Tools\Roslyn，从而让引擎具有了编译加密代码文件的功能

### Shader

### 资源

