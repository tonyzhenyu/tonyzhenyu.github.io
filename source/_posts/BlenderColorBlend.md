---
title: Blender 混合不同模型颜色
date: 2020/09/23 
tags: Shader, Blend
---

当两个网格相交时，添加动态材质混合和法线平滑以达到自然过渡。例如草和泥土与地面相交，沙坑与地面相交，山脉相交，悬崖相交等等。

![混合效果](https://devtalk.blender.org/uploads/default/optimized/2X/2/2c50dcbf0b48f83729a978eee6284bb1569835d7_2_690x290.jpeg)


为了实现以上实时混合的效果，查阅了相关资料，并且思考是如何在blender中实现。
![以下是我实现的效果](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE7248ef4d9b27f41275c4e62d415327b9/12)
![效果2](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE037d3a3b52ac6cdfe128ce43790b9343/20)

### 混合不同模型颜色

Blender 的底层是OpenGL底层代码会跟大多数主流游戏引擎稍有区别。
blender中修改模型数据并不会出现在Node Editor中，工作原理会跟游戏引擎中的渲染管线稍有区别，并且要做到修改模型数据，就得走blender中提供的模型数据修改器的一套操作。


### 若要进行实时修改需要对物体添加以下修改器：

    VertexWeightProximity
    DataTransfer
    Dynamic Paint

### 需要新建的物体信息：

    vertex group : ProximityVertex
    vertex color : PaintMap

### 需要的物体：
一个地形，一个需要进行混合的物体。

原理简述：工作原理是将石头作为画板，将地板作为笔刷，Dynamic Paint组件实时调用笔刷和画布将对应的画布模型进行顶点颜色绘制。每次绘制产生一张纹理，运行时动态放到画布物体的顶点颜色上，其值为01空间内运算。拿到绘制贴图后可以在节点修改器中添加各种操作。例如贴图高度混合，渐变过滤等等。有兴趣可以进一步研究。

## 关键步骤：
1. 创建两个物体（地形、混合物体）
2. 给地形添加 动态绘制修改器，并且把地形的动态绘制属性调整为笔刷
   ![笔刷](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCEeb128a28f2b8e9388391a918fba6ddec/27)
3. 给混合物体添加动态绘制修改器，把混合物体的动态绘制属性调整为画布，并且设置输出纹理到顶点颜色 PaintMap中
   ![画布](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE2ecea1644a0ee3887f3c462af05021c4/29)
4. 接着修改混合物体的法线，添加VertexWeightProximity，DataTransfer 修改器（注意修改器的顺序），并且把修改器参数调整为如下图参数。
   ![修改法线](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE1af9746e3459c87b45e64e44be9b9827/33)
   ![修改法线2](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCEed9b489c2866074a3386cc37b719790a/31)
   ![修改法线3](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE4fe5b2ef8f01333c48a496b4edb52379/23)
5. 下一步在节点编辑器中新建材质，新建两个着色器，使用mix shader 进行混合，把VertexColor当作MixShader的混合值进行混合
   ![材质节点](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/AB86CD3B14774EB3A89D08CF8EE43A55/39)
6. 最后在按照自己的要求新增一些节点就可以完成了
    ![效果1](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCEa48fd395ac6e7fe478fc92cd5435b034/14)
### 添加了高度混合的物体混合
TADAA!!
![效果2](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCEe3b9a00dbd4e5595f777e08c4af8d029/17)
![效果3](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCE037d3a3b52ac6cdfe128ce43790b9343/20)
![效果4](http://note.youdao.com/yws/public/resource/44930ab81b34013e973e4f9853746df8/xmlnote/WEBRESOURCEa0d620afd9b40ae892ea939285b7febc/25)
注解：VertexWeightProximity，DataTransfer 修改器顺序不能混淆的原因是，VertexWeightProximity把顶点权重绘制到对应的顶点组中，而DataTransfer 是读取对应的顶点权重进行遮挡。

修改器中的相关信息不在此详细描述，有关资料可以查阅blender的API文档。

## 已知限制
+ 不能实时读取对应的混合材质，只能放到相应的着色组上进行关联。


