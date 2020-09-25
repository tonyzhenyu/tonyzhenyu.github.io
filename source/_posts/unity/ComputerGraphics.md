---
title: 图形学自我总结
date: 2020/8/4
tags: [shader,unity]
---
<style type="text/css">
r {color:red; font-weight: 700;}
yellow {color: yellow ; font-weight: 700;}
g {color:green;font-weight: 700;}
</style>

# 我学废了！

总结一些关于实时渲染的坑和知识，学废的请举手！

---
## 光照模型 光影不分家
+ 冯氏光照模型
+ 兰伯特光照模型
+ 半兰伯特光照模型
+ 基于物理渲染
+ IBL基于图片光照着色
+ 利用渐变纹理实现伪BRDF
---
## shadow 阴影
+ 自定义阴影贴图
+ percentage closed soft shadow(pcss 阴影)
+ 方差阴影
---
## 变换 Transform 
+ Vertex Offset 顶点变换
---
## 纹理贴合 Texturing 
+ <g>world align textrue(xyz) 基于世界坐标的无限延展贴图</g>
+ <g>height base blend texture 基于高度的材质混合</g>
+ <r>view position parallex 基于视图空间的视差效果</r>
---
## 反射 Reflection 
+ matcap 材质捕获 三维空间与二维空间之间的转换 低配cubemap，平面反射中要进行法线重置
+ cubemap 静态的cubemap反射，贴图效果比matcap好但是贴图体积大

---
## 透明相关
+ Transparency 透射效果
+ Refraction 折射效果
+ 渲染队列
+ Alpha Test (透明裁剪)
+ Alpha Blend(透明融合)
---
## map相关
+ <r>maskmap </r>
+ <g>RGBA channel mask 单通道贴图控制</g>
+ ramp map 渐变式贴图
+ <r>splatmap 地形材质混合</r>


---
## 后处理相关
+ NDC重建世界坐标
+ 屏幕空间深度图