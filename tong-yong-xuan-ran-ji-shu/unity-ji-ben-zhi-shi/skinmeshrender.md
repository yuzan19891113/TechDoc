# Skinmeshrender

## GPUSkinning

因为移动端GPU的性能更吃紧，gpu skinning占用较多的GPU负载，而CPU有更多的余力，所以skinning在手机端会放在CPU做，并且使用GPU skinning部分机型有兼容性问题

## CPU Skinning

### **SkinnedMeshRender:UpdateSkinnedMeshes/SkinMeshImmediate**

{% hint style="info" %}
Schedule jobs to Job system
{% endhint %}

#### **SkinnedMeshRenderer:**PrepareSkin:准备骨骼索引，与骨骼矩阵

SkinnedMeshRendererManager::CalculateSkinningMatrices

```text
        PreparedRendererInfo& info = *m_PreparedInfos[renderer.m_Handle];
        TransformHierarchy* hierarchy = info.hierarchy;
```

{% hint style="info" %}
preparedRendererInfo来源于render，提供了层级的transform
{% endhint %}

```text
Matrix4x4f* __restrict outPoses = skinnedPoses;
for (int i = 0; i < size; ++i)
   outPoses[i] = float4x4ToMatrix4x4f(
   mul(tempTransforms[info.boneTransformIndices[i]], Matrix4x4fTofloat4x4(bindPoses[i])));
```

{% hint style="info" %}
TempTransforms是更新的动画矩阵，bindPose是记录mesh相关的骨骼初始状态的矩阵，相乘才得到乘以顶点的矩阵
{% endhint %}

### \*\*\*\*

### **MeshSkinning**:DeformSkinnedMesh:

{% hint style="info" %}
真正的cpu skinning阶段\(job\),发生在lateUpdate之后
{% endhint %}

### 

