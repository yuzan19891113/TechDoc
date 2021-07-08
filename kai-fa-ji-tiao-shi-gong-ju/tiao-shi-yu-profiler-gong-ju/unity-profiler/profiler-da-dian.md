# Profiler打点

## c\#层级增加profiler tag

Profiler.BeginSample

Profiler.EndSample\(\)



## C++层增加profiler tag

### 声明Profiler

PROFILER\_INFORMATION\(gShaderParseProfile, "Shader.Parse", kProfilerRender\);

### 使用Profiler

#### 插入代码段

PROFILER\_BEGIN\(gShaderParseProfile）PROFILER\_END\(gShaderParseProfile）

#### 插入整个函数

PROFILER\_AUTO\(gShaderParseProfile, this\);//scope 内统计

