# Unity 2019 insert profiler tag

#### 声明Profiler

PROFILER\_INFORMATION\(gShaderParseProfile, "Shader.Parse", kProfilerRender\);

#### 使用Profiler

PROFILER\_AUTO\(gShaderParseProfile, this\);//scope 内统计

