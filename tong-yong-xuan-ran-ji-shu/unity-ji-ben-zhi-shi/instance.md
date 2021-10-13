# Instance

Unity Material Instance

#### 使用constant buffer用来填充instancebuffer

InstancingBatcher:RenderInstance

#### 使用jobsystem填充instancebuffer

InstancingBatcher::FillInstanceBufferWithJob

#### InstanceBuffer来源于ShareMaterialData

batchRenderer:ApplyShaderPass

#### ForwardRenderLoop.cpp来收集每个Pass的Instance

ForwardRenderLoop.batchRenderer:ApplyShaderPass

derLoop..batchRenderer.Add(instance);

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

#### Instance打断的原因

ForwardRenderLoopJob.











