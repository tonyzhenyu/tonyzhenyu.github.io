---
title: 简单光照模型和阴影匹配
date: 2020/9/4
tags: [shader,unity]
categories: 
- Graphics

---
<style type="text/css">
r {color:red; font-weight: 700;}
g {color:green;font-weight: 700;}
y {color:yellow;font-weight: 700;}
</style>

# 兰伯特光照模型

## 简单光照模型和阴影匹配
光照模型和阴影匹配是在基础渲染中非常重要而且不可缺失的一部分。
### 兰伯特光照模型：
#### `lambert = max(0,dot(N,L))`

### 半兰伯特光照模型:
#### `lambert = dot(N,L) * 0.5 +0.5`

### RimLight边缘光
#### `Rim = View * Vertex.Normal`

用于作为渐变纹理的索引会是这样：
`uv = float2(Rim,Rim);`

调整后的曲线：

### 实现光照:

需要以下的核心操作
- 包含库 Lighting.cginc
- 包含库 UnityCG.cginc
- 顶点着色器提供 worldNormal
- 顶点着色器提供 worldPos

暴露的参数：
- `_DiffuseColor`
- `_SpecularColor`
- `_GlossColor`
  
外部调用的：
- `_LightColor0`
- `_WorldSpaceCameraPos`
- `UNITY_LIGHTMODEL_AMBIENT`

渲染路径会用到ForwardBase和ForwardAdd
- 通俗的逐顶点<g>ForwardBase 会计算平行光和环境光逐顶点/SH光照和Lightmaps</g>
- 通俗的逐像素<y>ForwardAdd 会计算额外的逐像素光源，每个Pass对应一个光源</y>

### 只考虑平行光的情况下简单光照模型的基础实现是：

    fixed3 worldNormal = normalize(i.worldNormal);
	fixed3 worldLightDir = normalize(_WorldSpaceLightPos0.xyz);

	fixed3 ambient = UNITY_LIGHTMODEL_AMBIENT.xyz;
    fixed3 viewDir = normalize(_WorldSpaceCameraPos.xyz - i.worldPos.xyz);

    fixed3 diffuse = _LightColor0.rgb * _Diffuse.rgb * max(0, dot(worldNormal, worldLightDir));	
    fixed3 halfDir = normalize(worldLightDir + viewDir);
    fixed3 specular = _LightColor0.rgb * _Specular.rgb * pow(max(0, dot(worldNormal, halfDir)), _Gloss);

    fixed atten = 1;
    fixed4 finnalColor = fixed4(ambient + (diffuse + specular) * atten, 1.0);

### 考虑其他光照情况下的光照模型是：
通常会放在逐像素的渲染路径下

    #ifdef USING_DIRECTIONAL_LIGHT
				fixed atten = 1.0;
	#else
        //点光源的光照计算 通常会用在逐像素的光照路径下
		#if defined (POINT)
	        float3 lightCoord = mul(unity_WorldToLight, float4(i.worldPos, 1)).xyz;
	        fixed atten = tex2D(_LightTexture0, dot(lightCoord, lightCoord).rr).UNITY_ATTEN_CHANNEL;
        //聚光灯的光照计算 通常会用在逐像素的光照路径下
	    #elif defined (SPOT)
	        float4 lightCoord = mul(unity_WorldToLight, float4(i.worldPos, 1));
	        fixed atten = (lightCoord.z > 0) * tex2D(_LightTexture0, lightCoord.xy / lightCoord.w + 0.5).w * tex2D(_LightTextureB0, dot(lightCoord, lightCoor.rr).UNITY_ATTEN_CHANNEL;
	    #else
	        fixed atten = 1.0;
	    #endif
	#endif

光照计算完成之后，效果只有光线没有影子，这时候需要捕获unity渲染好的影子叠加到渲染结果上。

## 影子的实现：
影子实现需要的包含库文件：
- `autolight.cginc`
- `lighting.cginc`
- `UnityCG.cginc`


    struct v2f
    SHADOW_COORDS(1)    //宏表示为定义一个float4的采样坐标，放到编号为1的寄存器中       
    v2f vert (appdata_base v)           
    TRANSFER_SHADOW(o)  //根据变换求解上面结构体中的float4坐标，unity5中采用的是屏幕空间阴影贴图        
    fixed4 frag (v2f i) : SV_Target
    fixed shadow = SHADOW_ATTENUATION(i); //根据贴图与纹理坐标对纹理采样得到shadow值。
            
获取到的shadow 其中可以做任何操作，最终叠加到渲染结果上。

所以最后的光照加上影子的运算结果应该是：

    finalcolor = difuse * shadow + specular
        
以后可以做一些简单的逐像素或者逐顶点的通用着色器了。

可以做成通用着色器后做可复用的叠加的一些效果。