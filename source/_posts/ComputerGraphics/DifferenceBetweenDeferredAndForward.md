---
title: 渲染路径 Deferred Forward 区别
date: 2020/9/25
tags: [shader,render]
categories: 
- Graphics

---

# 渲染路径的区别

两个渲染路径本身结构。

## Forward
PerLight
Depth
ShadowCaster
DrawMesh

## Deferred

G-Buffer
    Albedo
    Specular
    Roughness
    Normal
PerObject