# Unity Player Loop

## PlayerLOOP

PLAYER\_LOOP\_INITIALIZATION

PLAYER\_LOOP\_EARLY\_UPDATE

PLAYER\_LOOP\_FIXED\_UPDATE

PLAYER\_LOOP\_PRE\_UPDATE

PLAYER\_LOOP\_UPDATE

PLAYER\_LOOP\_PRE\_LATE\_UPDATE

PLAYER\_LOOP\_POST\_LATE\_UPDATE



## 插入Callbacks

```cpp
PLAYER_LOOP_INJECT(DynamicBoneLateUpdate)
```

{% code title="playerLoopCallback.h" %}
```cpp
#define PLAYER_LOOP_POST_LATE_UPDATE \
    PLAYER_LOOP_INJECT(PlayerSendFrameStarted) 
                                               
    PLAYER_LOOP_INJECT(DirectorLateUpdate) \
    PLAYER_LOOP_INJECT(ScriptRunDelayedDynamicFrameRate)\
    PLAYER_LOOP_INJECT(DynamicBoneLateUpdate) \
    PLAYER_LOOP_INJECT(PhysicsSkinnedClothBeginUpdate) \
    PLAYER_LOOP_INJECT(UpdateRectTransform) \
    PLAYER_LOOP_INJECT(PlayerUpdateCanvases) \
    PLAYER_LOOP_INJECT(UpdateAudio) \
    PLAYER_LOOP_INJECT(VFXUpdate) \
    PLAYER_LOOP_INJECT(ParticleSystemEndUpdateAll) /*  We need to sync particle systems here to */ \
                                                   /* make sure they update their renderers properly */ \
    PLAYER_LOOP_INJECT(EndGraphicsJobsAfterScriptLateUpdate) /* Latest possible time to end graphics jobs of the previous frame. Must run before any graphics callbacks. */ \
    PLAYER_LOOP_INJECT(UpdateCustomRenderTextures) \
    PLAYER_LOOP_INJECT(UpdateAllRenderers) \
    PLAYER_LOOP_INJECT(EnlightenRuntimeUpdate) \
```
{% endcode %}

{% hint style="info" %}
插入一个Callback在PostLateUpdate,注意插入顺序代表执行顺序DynamicBoneLateUpdate执行在PhysicsSkinnedColthBeginUpdate之前
{% endhint %}



