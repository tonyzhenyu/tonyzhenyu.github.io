---
title: unity的经验（杂）
date: 2020/8/3
tags: [shader , unity]
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


[47张map](https://zhuanlan.zhihu.com/p/27339998)
---
### 在amplify shader editor 里的坑

如果需要转换以上矩阵，应该使用transform 节点进行转换为最好


---

unity 读取文件时
    Assestdatabase.LoadAssetsAtPath<Type>("Assets/..");
    Resources.Load<Type>("Assets/Resources/..");