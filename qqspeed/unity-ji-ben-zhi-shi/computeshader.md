# ComputeShader

1. 命名为compute
2. RW的buffer为computebuffer
3. computebuffer.getdata可以返回数据

### 采样texture

线程数固定成32 \* 32，那么dispatch的线程组数为width / 32, height/32，那么SV\_DispatchThreadID的xy刚好可以索引一一对应的像素。

**.cs**

```text
                uint[] BackData = new uint[4]{0, 0, 0, 0};
                if (ResultBuffer == null)
                {
                    ResultBuffer = new ComputeBuffer(4, 4);        
                }
                ResultBuffer.SetData(BackData);      
                int xGroups = width / 32;
                int yGroups = height / 32;
                  
                _CSShader.SetBuffer(kernel, "_ResultBuffer", ResultBuffer);
                _CSShader.Dispatch(kernel, xGroups, yGroups, 1);

                ResultBuffer.GetData(BackData);
```

**.compute**

```text
#pragma kernel ComputeDiffTotalAndAvg

//[0]:count [1]: Total_L [2] Total_a, [3] Total_b
RWStructuredBuffer<uint> _ResultBuffer;

Texture2D<float4> _GammaTex;
Texture2D<float4> _LinearTex;

float4 _CullThrehold;

//rgb2lab code miss

[numthreads(32, 32, 1)]
void ComputeDiffTotalAndAvg(uint3 inp : SV_DispatchThreadID)
{
	float3 GammaRes  = rgb2lab(_GammaTex[inp.xy].rgb);
	float3 LinearRes = rgb2lab(_LinearTex[inp.xy].rgb);
	
	float3 DiffRes = LinearRes - GammaRes;
	
	if (length(DiffRes) > _CullThrehold.w)
	{
		//transform to 0-255
		uint DiffX_Int = abs(DiffRes.x) * 255;
		uint DiffY_Int = abs(DiffRes.y) * 255;
		uint DiffZ_Int = abs(DiffRes.z) * 255;

		InterlockedAdd(_ResultBuffer[0], 1);
		InterlockedAdd(_ResultBuffer[1], DiffX_Int);
		InterlockedAdd(_ResultBuffer[2], DiffY_Int);
		InterlockedAdd(_ResultBuffer[3], DiffZ_Int);
	}	

}


```



