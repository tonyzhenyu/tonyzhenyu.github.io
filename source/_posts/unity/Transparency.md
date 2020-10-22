---
title : Transparency in Defered Lighting 
date : 2020/8/23
tags : [shader , unity]
categories: 
- Unity

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