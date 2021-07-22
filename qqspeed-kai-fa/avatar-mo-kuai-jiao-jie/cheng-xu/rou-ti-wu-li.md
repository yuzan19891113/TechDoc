# Avatar物理

## Dynamic Bone

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







## Cloth

