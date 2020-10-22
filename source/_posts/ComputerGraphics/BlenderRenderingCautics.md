---
title: Blender渲染玻璃焦散
date: 2020/10/22
tags: [Shader, Blend]
categories: 
- Graphics

---


![玻璃焦散](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3264915731,2306361361&fm=26&gp=0.jpg)

&emsp;&emsp;焦散是指当光线穿过一个透明物体时，由于对象表面的不平整，使得光线折射并没有平行发生，出现漫折射，投影表面出现光子分散。
焦散，俗称“水光”，波光粼粼---即使指焦散现象。
&emsp;&emsp;光谱中的颜色映射关系，在RGB上面的映射顺序分别是蓝绿红。
![光色关系](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603365753747&di=87de1b4ca97243920dac904d27b76df3&imgtype=0&src=http%3A%2F%2Fwww.microsenso.com%2Fueditor%2Fphp%2Fupload%2Fimage%2F20161110%2F1478756299163735.png)

# 最终实现结果
EEVEE中的效果:
![render3.png](https://i.loli.net/2020/10/22/KuzVAkxqomZhNIr.png)
Unity中的效果:
![a.jpg](https://i.loli.net/2020/10/22/c2a5opzVLqvwghS.jpg)

&emsp;&emsp;更高级的效果可以利用`RenderTexture`渲染，求各顶点到投射平面的交集做插值。

# 利用光照结果制作
制作解析，主要利用光照穿过物体产生影子，利用物体中`Dot(Normal,LightDir)`做渐变图的索引值，把需要穿透的面剔除，把需要留下的面保留。而利用内部光照从内往外照射产生对应的影子制作出焦散。

制作基础的光照系统：
![cauticsa.png](https://i.loli.net/2020/10/22/IBaeWlwcuiPENr9.png)


## 材质
- 主要属性：设置透明队列为Alpha Hashed，shadow Mode 为Alpha Hashed
- 节点：主要把阴影区域剔除，剩下的交给shadow Caster
![cauticsb.png](https://i.loli.net/2020/10/22/Uzrbvo9PupQTN26.png)

