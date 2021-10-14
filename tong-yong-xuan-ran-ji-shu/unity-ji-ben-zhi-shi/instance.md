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
     batchrender.flush()//render instance
   end
   batchrender.ApplyShaderPass();
   batchRenderer.add(instance)
end
batchRenderer.EndLoopFlush();//render instance
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

BatchRenderer:Flush

BatchRenderer:RenderBatch

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
//拷贝材质shader相关的 props
memcpy dest_instancebuffer src_instanceMaterial
    memcpy dest_instancebuffer src_instanceMaterial_defaultProps(材质share部分)
    memcpy dest_instancebuffer src_instanceMaterial_NodeProps(材质不share部分)
end
```

### J3差异

instance收集方式并没有变化

InstancedPropInfo::RenderVaInstances

使用VBO bufferqueue,除了支持instance数量更大，还有没有其他优势

unity2019使用jobs去填充instancedata，j3没有实现

填充的instancedata unity2019已经包含sh, lightmapst，lodfad

J3的instanceData包含有dynamicLOD，，需要取当前lod的materialinfo,每个节点有多个lod nodeinfo

###  计划：

















