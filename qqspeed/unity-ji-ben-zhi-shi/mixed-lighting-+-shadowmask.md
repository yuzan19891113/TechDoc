# Mixed lighting + shadowmask

static object =&gt;  bake result : shadowmask +  lightmap + lightprobe + reflectionProbe

#### static object: macro: LIGHTMAP\_ON on / LIGHTPROBE\_SH off

**shader:**  

dirlight.diffuse =  light.color \* bake.shadowmask.r \* N; 

dirlight.specular =   light.color \* bake.shadowmask.r \* \(R \* N\)

indirectLight.Diffuse = bake.lightmap.r; 

indirectLight.specular = bake.reflectionProbe.r ;



#### dynamic object: macro: LIGHTMAP\_ON off / LIGHTPROBE\_SH on

dirlight.diffuse =  light.color \* bake.RealTimeshadowmask.r \* N; 

dirlight.specular =   light.color \* bake.RealTimeshadowmask \* \(R \* N\)

indirectLight.Diffuse = bake.lightProbe.r

indirectLight.specular = bake.reflectionProbe.r









