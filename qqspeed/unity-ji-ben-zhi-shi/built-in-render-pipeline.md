# Built-in render pipeline



#### LightMode tag

　　渲染路径标签， 一般现在渲染分为了 三类，顶点渲染路径，向前的渲染，对于的延迟渲染路径。

* Always: Always rendered; no lighting is applied.
* ForwardBase: Used in Forward rendering, 　　ambient, main directional light, vertex/SH lights and lightmaps are applied. 只受到环境光，主要（强度最大那个）的方向光，球协光照和lightMap影响
* ForwardAdd: Used in Forward rendering; additive per-pixel lights are applied, one pass per light. 如果灯光类型是 NO-IMPORT 或者其他类型光源 就会用到这个
* Deferred: Used in Deferred Shading; renders g-buffer. 延迟渲染的，渲染到Gbuffer
* ShadowCaster: Renders object depth into the shadowmap or a depth texture. 生成阴影要用深度图shader
* MotionVectors: Used to calculate per-object motion vectors. 计算物件移动向量
* PrepassBase: Used in legacy Deferred Lighting, renders normals and specular exponent. 
* PrepassFinal: Used in legacy Deferred Lighting, renders final color by combining textures, lighting and emission.
* Vertex: Used in legacy Vertex Lit rendering when object is not lightmapped; all vertex lights are applied.
* VertexLMRGBM: Used in legacy Vertex Lit rendering when object is lightmapped; on platforms where lightmap is RGBM encoded \(PC & console\).
* VertexLM: Used in legacy Vertex Lit rendering when object is lightmapped; on platforms where lightmap is double-LDR encoded \(mobile platforms\).

