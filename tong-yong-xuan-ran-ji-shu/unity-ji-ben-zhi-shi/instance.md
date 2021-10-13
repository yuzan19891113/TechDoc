# Instance

## Unity Material Instance

### ForwardRenderLoop.cpp

batchrender:负责动态合批与instancing

InstanceBatcher:真正的instance render

### instance count

InstancingBatcher::BuildFrom  决定instance 的数量m_BatchSize

可以通过m_UseFlexibleArraySize， gMaxFlexibleArrayBatchSize来限制

### Instance收集

ForwardRenderLoop.ForwardRenderLoopJob

伪代码

```
//instance collect
for each pass do
   if batch breaks
     batchrender.flush()
   end
   batchrender.ApplyShaderPass();
   batchRenderer.add(instance)
end
batchRenderer.EndLoopFlush();
```

同时lightmap, SH也进入到了instance

```
 const BuiltinShaderParamValues& builtins = GetGfxDevice().GetBuiltinParamValues();
            if (m_InstancingBatcher.HasSHCoeffsArrays())
                PushVector4fToBatchInstanceDataArray(m_BatchInstances, &builtins.GetVectorParam(kShaderVecSHAr), SphericalHarmonicsL2::kVec4Count);
            if (m_InstancingBatcher.HasProbesOcclusionArray())
                PushVector4fToBatchInstanceDataArray(m_BatchInstances, &builtins.GetVectorParam(kShaderVecProbesOcclusion), 1);
            if (m_InstancingBatcher.HasLightmapSTArrays())
                PushVector4fToBatchInstanceDataArray(m_BatchInstances, &builtins.GetVectorParam(kShaderVecUnityLightmapST), kLightmapTypeCount);
```

### Instance绘制

InstancingBatcher:RenderInstance

使用常量Buffer来存储instancebuffer

```
MapConstantBuffers();
FillInstanceBufferWithJob();
UnMapConstanceBuffer();
```

使用jobsystem填充instancebuffer

InstancingBatcher::FillInstanceBufferWithJob

#### 伪代码

```
//拷贝built in props, etc lightmapST, SH, worldmatrix
memcpy dest_instancebuffer src_instancedata
//拷贝材质shader相关的 props(share material data,)
memcpy dest_instancebuffer src_instanceMaterial
    memcpy dest_instancebuffer src_instanceMaterial_defaultProps(材质share部分)
    memcpy dest_instancebuffer src_instanceMaterial_NodeProps(材质不share部分)
end
```

















