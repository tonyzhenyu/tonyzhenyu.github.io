---
title: 湖中小屋
date: 2020/11/1
tags: [shader,render]
categories: 
- Graphics

---

![render-32.png](https://i.loli.net/2020/11/01/hkarEnfseRt4lvm.png)

---
故事简介：
&emsp;&emsp;湖中小屋
&emsp;&emsp;这是一个我突发奇想的项目，在pintrest中找到了许多关于湖中岛的一些参考。按照想法中的故事情节或许是湖中荒废的岛屿，岛屿上有一户人家，喜欢独居。

---
<p style ="color: green;">项目规格:</p>

- 贴图上限2048*2048
- 贴图格式png
- 观察距离1.5米内
- 使用到模型顶点权重

---


![lakecabin_ho'sref](https://i.loli.net/2020/11/02/fqO8ILxGiX4UMjE.jpg)
参考图


---
# 完成的资源
![nonlight view.png](https://i.loli.net/2020/11/01/qNHodGlsRPp2E8V.png)
![lakecabin_ho_sresources.jpg](https://i.loli.net/2020/11/02/MabQSVWOoHv3rBg.jpg)

汇总：

- 树
  - 灌木丛
  - 草
  - 落叶
- 石头
- 地形
- 雾
- 房子
- 水面

树：树干模型使用curve生成，利用树干的顶点权重生成对应的枝叶，生成数量为50，并且修改枝叶的法线投射到圆球上。
灌木丛：灌木丛模型，灌木丛使用跟树木一样的原理，唯一的不用就是生成数量减少。
草：面片模型，`alpha clip` 做透明裁剪，`transparency`做透射
落叶：面片弯曲模型，`alpha clip` 做透明度裁剪，`transparency`做透射
石头：石头模型，使用`mesh volume proximity` 做顶点权重，`data transfer`做法线传递。材质通过高度图混合。
地形：地形材质，使用`vertex position `的z轴进行映射做通道蒙版。
雾：`volume `材质，使用`object space mapping `做`noise texture`中的纹理uv，并且通过扰乱原本的uv做非平均特征体积雾。
房子：手动建模，优化方案：使用更加适合布尔切割的软件或者命令。
水面：水面的材质使用`transmission `属性做透射，使用`alpha hashed `做透明通道，水面反射使用roughness和平面反射辐射盒。水面的模型使用了海面修改器直接生成海面。

---

# 整合后

无全局光参与：
![lakecabin_ho'srender-32-nonlight2](https://i.loli.net/2020/11/02/aH8smdUOJg6ET2W.jpg)

全局光照明：
![lakecabin_ho_srender-32](https://i.loli.net/2020/11/02/1K5Lr3iwjSNfgks.jpg)

---

# 总结
&emsp;&emsp;blender中cycles的透明渲染是使用光线的采样数，光线采样数越低，z-depth的穿透能力越低，具体表现在z-depth通道在两个模型之后莫名变白。
&emsp;&emsp;blender中eevee使用到TAA的降噪技术，具体表现在直接采样数，采样数越高，合成样张越多，TAA通过每次查找上一样张的相同处和不同处进行合成，最终得出噪点较少的画面，同样也可用于抗锯齿。
&emsp;&emsp;渲染树木的最主要特点是更改树叶的直接法线，并且因为树叶使用模型空间的uv坐标，所以对贴图中的树叶大小把握要非常恰当，否则会出现比例不符合的现象。

---

各种不同的角度
![lakecabin_ho'srender-33]https://i.loli.net/2020/11/02/gxA3Qn5iKHSqWF2.jpg
![lakecabin_ho'srender-38](https://i.loli.net/2020/11/02/SALY9myRkcws4eC.jpg)
![lakecabin_ho'srender-36](https://i.loli.net/2020/11/02/nSJO81ztkDfFHTb.jpg)
![lakecabin_ho'srender-40](https://i.loli.net/2020/11/02/fgQjdMopb9XuKBz.jpg)
![lakecabin_ho_srender-34](https://i.loli.net/2020/11/02/QwyiHUFjz54fgE6.jpg)
![lakecabin_ho'srender-37](https://i.loli.net/2020/11/02/yKjAhmzqe1rVG29.jpg)
