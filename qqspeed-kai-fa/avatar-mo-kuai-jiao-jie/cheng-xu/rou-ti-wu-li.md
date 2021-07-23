# Avatar物理

## Dynamic Bone

[https://www.jianshu.com/p/de3a86cfb129](https://www.jianshu.com/p/de3a86cfb129)

每一个subMesh都包含若干了DynamicBone,每一个DynamicBone都包含一个骨骼链

![](../../../.gitbook/assets/image%20%28222%29.png)

### Update monobehaviour

### Update As JobSystem

```text
dynamicboneutility
{
    struct DynamicBoneJobData;
    typedef vector_map<int, DynamicBoneJobData*> ActiveDynamicBoneJobMap;
    static ActiveDynamicBoneJobMap s_ActiveDynamicBoneJobMaps;
}
```

每一个DynamicBoneJobData都是一根独立的骨骼链

#### DynamicBoneUtility:DoJob\(run at PostLateUpdate\)

ApplyParticlesToTransforms: **FetchResults**

ScheduleForEach: start dispatch jobs to jobsystem\(**start** **Simulate**\)

#### DynamicBoneUtility:CustomLateUpdateAsJob

create or modify DynamicBoneJobData,DynamicBoneJobData 包含collider,gravity\(**update render to dynamics**）

#### DynamicBoneUtility:UpdateDynamicBoneEachJob

apply parent move offset to dynamicbones

```text
    *m_ObjectScale = Abs(m_RootTransform->GetWorldScaleLossy().x);
    *m_ObjectMove = m_RootTransform->GetPosition() - *m_ObjectPrevPosition;
    *m_ObjectPrevPosition = m_RootTransform->GetPosition();
```

 





## Cloth

