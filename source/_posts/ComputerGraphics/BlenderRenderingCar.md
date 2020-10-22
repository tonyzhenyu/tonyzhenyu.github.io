---
title: Blender渲染汽车Mazda rx7
date: 2020/10/22
tags: [Shader, Blend]
categories: 
- Graphics

---

![Mazdarx-7](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603360275843&di=c4af824e2ae1a25c1c5765131d94dd6e&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn14%2F289%2Fw657h432%2F20180805%2F755b-hhhczfa8313868.jpg)

# Mazda RX-7 
&emsp;&emsp;Mazda Rx-7是我比较喜欢的跑车之一，修长的车头和圆润的造型以及流线型的车身，非常适合各种改装手段，汽配城改装车小有名气的车也就是rx7。
&emsp;&emsp;RX-7是MAZDA旗下的跑车，日本跑车中代表性的一款，RX-7采用传统跑车标准的FR驱动配置，搭载五速手排变速箱，车尾造型一体化尾灯组是全车识别性最高的地方。
&emsp;&emsp;MAZDA RX-7与NSX、Supra、3000GT一样同为日本高性能化跑车的代表，但是由于RX-7略嫌诡异的身材、还有以转子心脏跻身性能跑车之林的剽悍本色，在大部分车迷的眼中，这部车一直是较另类的角色，是《假面骑士black rx》里面莱多隆战车的原型，也是《名侦探柯南》中安室透的爱车。

# 模型来自于sketchfab
&emsp;&emsp;拿到的模型并不是直接能用的，经过了一系列的修改后才能够正常使用，优点是材质区分的不错，不过就是太多。
&emsp;&emsp;整个模型的拓扑不是最好的，经过细分后明显能检查到断面和一些材质过渡区的材质误识别，修改拓扑还是关键。
&emsp;&emsp;整体模型的分层做的不错，但是模型的观察尺度只能停留在远处。
![a1.png](https://i.loli.net/2020/10/22/GaqOPHy5XCVnrYv.png)

![a4.png](https://i.loli.net/2020/10/22/bWRePTfk8gLNqd3.png)
# 最终的渲染图片
![render4.png](https://i.loli.net/2020/10/22/xKUNQzyjM86LrV7.png)
![render5.png](https://i.loli.net/2020/10/22/HYimN7pWRqMIrPu.png)

## 材质
+ 车身材质：
  - 为了凸显材质的层次，使用了`clearcoat`。
  - 微平面使用与相机距离产生关系的噪波纹理用到`roughness`通道上。
  - 颜色使用`fresnel `反射映射到hsv的hue通道上，使车身颜色在不同的角度观察有不一样的颜色。
  - 尽量使车身保持光滑
+ 车灯材质：
  - 直接使用了法线贴图制作凹凸映射
+ 玻璃材质：
  - 透明`Alpha Hash`
  - 屏幕空间反射
+ 轮毂材质：
  - 各向异性的金属`anistropic`

![a2.png](https://i.loli.net/2020/10/22/1DYkseqRVwuNQvb.png)


## 灯光
&emsp;&emsp;灯光使用了交叉的面光源作为顶光在整车上方往下投射，侧光源也使用的交叉面光源在左侧补光，装饰光源使用了点光源在车灯处进行点缀，三种灯光在不同的位置下建立的基础灯光系统。

## 场景氛围
- 雾气制造空气氛围（散射雾气）
- 纯黑天空无背景色（突出主体）
- 平面反射（blender中的平面反射只会作用在那些Roughness为0的材质上）
- 设置bloom的效果，阈值不要太低
![a3.png](https://i.loli.net/2020/10/22/7nJkcbYKQjGseVo.png)
![a.png](https://i.loli.net/2020/10/22/31eSAz6pgGbRfqr.png)

## 镜头
&emsp;&emsp;镜头选择了88焦段的`长焦镜头`，用于远处拍摄整车，并且没有选择虚焦的效果，因为在eevee上的虚焦表现可能不尽人意，效果并不太满意。
