# bake light mapping

A Meta Pass is a **Shader** pass that provides albedo and emission values to the Progressive **Lightmapper**  
 , so that they can correctly compute indirect lighting from that **GameObject,**For a **GameObject** to work with [lightmapping](https://docs.unity3d.com/Manual/Lightmappers.html), the GameObject must use a Material that has a Meta Pass in its Shader.

The Meta Pass provides albedo and emission values in texture space. These values are separate from those used in real-time **rendering,** meaning that you can use the Meta Pass to control how a GameObject looks from the point of view of the lighting baking system without affecting its appearance at runtime. An example of when this would be useful is if you wanted the green moss on a cliff to generate exaggerated green indirect light in your **lightmaps**, but you didn’t want to recolor the **terrain**  
 in the real-time pass of shader.

All of Unity’s built-in Materials have a Meta Pass, and the **Standard Shader** contains a Meta pass. If you are using these, you do not need to do anything to enable the Meta Pass. If you are using a custom Shader, you can add your own Meta Pass.

**烘培的 lightmap需要考虑abedo**



```text
Shader "Custom/metaPassShader"{
Properties {
    _Color ("Color", Color)=(1,1,1,1)
    _MainTex ("Albedo (RGB)",2D)="white"{}
    _Glossiness ("Smoothness", Range(0,1))=0.5
    _Metallic ("Metallic", Range(0,1))=0.0

    _GIAlbedoColor ("Color Albedo (GI)", Color)=(1,1,1,1)
    _GIAlbedoTex ("Albedo (GI)",2D)="white"{}
}

SubShader {
// ------------------------------------------------------------------
// Extracts information for lightmapping, GI (emission, albedo, ...)
// This pass is not used during regular rendering.
    Pass
    {
        Name "META"
        Tags {"LightMode"="Meta"}
        Cull Off
        CGPROGRAM

        #include"UnityStandardMeta.cginc"

        sampler2D _GIAlbedoTex;
        fixed4 _GIAlbedoColor;
        float4 frag_meta2 (v2f_meta i): SV_Target
        {
            // We're interested in diffuse & specular colors
            // and surface roughness to produce final albedo.

            FragmentCommonData data = UNITY_SETUP_BRDF_INPUT (i.uv);
            UnityMetaInput o;
            UNITY_INITIALIZE_OUTPUT(UnityMetaInput, o);
            fixed4 c = tex2D (_GIAlbedoTex, i.uv);
            o.Albedo = fixed3(c.rgb * _GIAlbedoColor.rgb);
            o.Emission = Emission(i.uv.xy);
            return UnityMetaFragment(o);
        }

        #pragma vertex vert_meta
        #pragma fragment frag_meta2
        #pragma shader_feature _EMISSION
        #pragma shader_feature _METALLICGLOSSMAP
        #pragma shader_feature ___ _DETAIL_MULX2
        ENDCG
    }

    Tags {"RenderType"="Opaque"}
    LOD 200

    CGPROGRAM
    // Physically-based Standard lighting model, and enable shadows on all light types
    #pragma surface surf Standard fullforwardshadows nometa
    // Use Shader model 3.0 target, to get nicer looking lighting
    #pragma target 3.0

    sampler2D _MainTex;

    struct Input {
        float2 uv_MainTex;
    };

    half _Glossiness;
    half _Metallic;
    fixed4 _Color;

    void surf (Input IN,inout SurfaceOutputStandard o){
        // Albedo comes from a texture tinted by color
        fixed4 c = tex2D (_MainTex, IN.uv_MainTex)* _Color;
        o.Albedo = c.rgb;
        // Metallic and smoothness come from slider variables
        o.Metallic = _Metallic;
        o.Smoothness = _Glossiness;
        o.Alpha = c.a;
    }
    ENDCG
}

FallBack "Diffuse"
```

}

