---
title: unity的经验（杂）
date: 2020/8/3
tags: [shader , unity]
categories: 
- Unity

---

在写shader或者制作shader graph 时候阅读shader代码经常会需要转译。

---

### 内置矩阵 支持的矩阵（float4x4）：

+ UNITY_MATRIX_MVP              当前模型视图投影矩阵
+ UNITY_MATRIX_MV               当前模型视图矩阵
+ UNITY_MATRIX_V                当前视图矩阵。
+ UNITY_MATRIX_P                目前的投影矩阵
+ UNITY_MATRIX_VP               当前视图*投影矩阵
<font color="lighgrey"> 
+ UNITY_MATRIX_T_MV             移调模型视图矩阵 
</font>

+ UNITY_MATRIX_IT_MV            模型视图矩阵的逆转
+ UNITY_MATRIX_TEXTURE0         纹理变换矩阵
    UNITY_MATRIX_TEXTURE3            
+ UNITY_LIGHTMODEL_AMBIENT      当前环境的颜色


---

### 在Amplify shader editor 里的坑

如果需要转换以上矩阵，应该使用transform 节点进行转换为最好


---

unity 读取文件时
    Assestdatabase.LoadAssetsAtPath<Type>("Assets/..");
    Resources.Load<Type>("Assets/Resources/..");

---


## 在defered lighting 中实现transparency 透射效果.

        //Work out where on screen this pixel in. This tells us where we need to read from in the light buffers

        float2 screenPos = input.PositionCS.xy / input.PositionCS.w;
        float2 texCoord = float2(
        	 (1 + screenPos.x) / 2 + (0.5 / Resolution.x),
        	 (1 - screenPos.y) / 2 + (0.5 / Resolution.y)
        );

        //The normal buffer alpha channel set to zero if nothing was written here. Sample the value as use zero-values as an early exit (a kind of stencil buffer)

        float4 sampledNormals = tex2D(normalSampler, texCoord);
        if (sampledNormals.a == 0)
            clip(-1);

        //This is the clever bit for back-face geometry. the depth of this vertex (input.Depth) is the back of the object...
        //...while the value in the depth buffer is the depth from the front of the geometry we rendered before into the gbuffer!
        //Thickness is easy to calculate.

        float backDepth = input.Depth;
        float frontDepth = tex2D(depthSampler, texCoord);
        float thickness = (backDepth - frontDepth) * (FarClip - NearClip);

        //This is the surface colour of the transparent object

        float3 opaque = tex2D(transparencyLightbufferSampler, texCoord).rgb;

        //This is the colour of the scene behind the object

        float3 background = tex2D(lightbufferSample, texCoord).rgb;

        //now blend them. Blending function varies per material

        return Blend(opaque, background, thickness);