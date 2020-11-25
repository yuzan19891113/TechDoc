# OnRenderImage

属于后期脚本需要重载的部分

需要添加tag,并且需要挂在camera同级目录下

```text
[ExecuteInEditMode]
[ImageEffectAllowedInSceneView]
public class EffectsToRT : MonoBehaviour
```

因为onRenderImage位于posteffect流程中，可能camera.render不会生效

