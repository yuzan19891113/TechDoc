# shadow

Unity 使用的是CSM\(cascade shadowmapping\)

1. render几个不同视锥裁剪面的depthrt,会保存为texuturearray
2.  screen space 的shadowcollect pass,通过主相机的depth texture恢复wpos,与shadowmap depth进行比较,得到shadowmask, 如果是cascade shadowmap,通过UNITY\_SETUP\_STEREO\_EYE\_INDEX\_POST\_VERTEX，来确定坐标采样哪一个textureArray中哪一个shadowmap
3. 采样shadowmap，得到shadow factor进行pixel light shading

Unity开启流程：

1. \_CameraDepthTexture和\_CameraDepthNormalsTexture是unity提供的内置纹理，将摄像机的depthTextureMode设置为Depth或DepthNormals即可以渲染这两张纹理。
2. 只要保证你的shader拥有一个ShadowCaster pass即可渲染到\_CameraDepthTexture
3. Custom shader要先加入一个LightMode为ShadowCaster的pass，例如：

   ```text
   // Pass to render object as a shadow caster
   Pass 
   {
   Name "ShadowCaster"
   Tags { "LightMode" = "ShadowCaster" }

       CGPROGRAM
       #pragma vertex vert
       #pragma fragment frag
       #pragma target 2.0
       #pragma multi_compile_shadowcaster
       #pragma multi_compile_instancing // allow instanced shadow pass for most of the shaders
       #include "UnityCG.cginc"

       struct v2f {
           V2F_SHADOW_CASTER;
           UNITY_VERTEX_OUTPUT_STEREO
       };

       v2f vert(appdata_base v)
       {
           v2f o;
           UNITY_SETUP_INSTANCE_ID(v);
           UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO(o);
           TRANSFER_SHADOW_CASTER_NORMALOFFSET(o)
           return o;
       }

       float4 frag(v2f i) : SV_Target
       {
           SHADOW_CASTER_FRAGMENT(i)
       }
       ENDCG
   }
   ```

   这个Pass把物体渲染进ShadowMap

4 在ForwardBase Pass中，加入

```text
Pass
{
    Tags { "LightMode" = "ForwardBase" }

    CGPROGRAM

    #pragma vertex vert
    #pragma fragment frag
    #include "UnityCG.cginc"

    #pragma multi_compile_fwdbase

    #include "AutoLight.cginc"


    struct v2f
    {
        float4 pos : SV_POSITION;
        float3 worldPos : Texcoord0;
        LIGHTING_COORDS(1,2)
    };


    v2f vert(appdata_base v) {
        v2f o;
        o.pos = UnityObjectToClipPos(v.vertex);
        TRANSFER_VERTEX_TO_FRAGMENT(o);

        return o;
    }

    fixed4 frag(v2f i) : COLOR {
        float atten = LIGHT_ATTENUATION(i);
        //UNITY_LIGHT_ATTENUATION(atten, i, i.worldPos);
        fixed3 outColor = fixed3(0.6, 0.0, 0.0) * atten;
        return float4(outColor, 0.5);
    }
    ENDCG
}
```

得到的atten 就是shadow值。

unity中基本会影响阴影的基本设置：  
![&#x5728;&#x8FD9;&#x91CC;&#x63D2;&#x5165;&#x56FE;&#x7247;&#x63CF;&#x8FF0;](https://img-blog.csdnimg.cn/20200505012141805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pzMDkwNw==,size_16,color_FFFFFF,t_70)  


![](../../.gitbook/assets/image%20%2853%29.png)

1.ForwardBase Pass中只处理平行光阴影，LightMap等，其他的灯光需要在ForwordAdd Pass中进行处理。

2.在顶点着色器中加入顶点变换的话，接受阴影需要使用UNITY\_LIGHT\_ATTENUATION\(atten, i, i.worldPos\);因为在设置阴影坐标的时候是把v.vertex变换到灯光空间，位移之后除非改动v.vertex，否则阴影坐标计算的是顶点变换前的。

3.PC平台和移动平台对阴影的默认设置不一样，比如PC平台默认是Cascaded Shadow开启，那如果物体想仅接收阴影，也必须写Shadowcaster Pass，因为使用的屏幕空间的Cascaded Shadow Map， 移动平台默认是关闭Cascaded Shadow，仅接收阴影不用写Shadowcaster Pass；具体可参见unity5.0更新说明，Shadowcaster实际是用来收集CameraDepthTexture：

