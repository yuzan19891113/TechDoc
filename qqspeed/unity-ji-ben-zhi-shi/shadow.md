# shadow



1. \_CameraDepthTexture和\_CameraDepthNormalsTexture是unity提供的内置纹理，将摄像机的depthTextureMode设置为Depth或DepthNormals即可以渲染这两张纹理。
2. 只要保证你的shader拥有一个ShadowCaster pass即可渲染到\_CameraDepthTexture
3. LightMode="ShadowCaster"

```text
//投射阴影的Pass 即本章节主题:ShadowCaster
		Pass
		{
			Tags{
				"LightMode" = "ShadowCaster"
			}
			CGPROGRAM
			#pragma vertex vert
			#pragma fragment frag
			#pragma multi_compile_shadowcaster
 
			#include "UnityCG.cginc"
 
			struct appdata {
				float4 vertex : POSITION;
				float4 uv : TEXCOORD0;
			};
 
			struct v2f {
				V2F_SHADOW_CASTER;
			};
 
			v2f vert(appdata v) {
				v2f o;
				TRANSFER_SHADOW_CASTER(o);
				return o;
			}
			fixed4 frag(v2f i) : SV_Target {
				SHADOW_CASTER_FRAGMENT(i)
			}
			ENDCG
		}

```



