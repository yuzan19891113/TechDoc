# Avatar物理

## Dynamic Bone

每一个subMesh都包含若干了DynamicBone,每一个DynamicBone都包含一个骨骼链

![](../../../.gitbook/assets/image%20%28221%29.png)

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

ScheduleForEach: start dispatch jobs to jobsystem\(**Simulate**\)

ApplyParticlesToTransforms: **FetchResults**

#### DynamicBoneUtility:CustomLateUpdateAsJob

create or modify DynamicBoneJobData

DynamicBoneJobData 包含collider,gravity

 





## Cloth

