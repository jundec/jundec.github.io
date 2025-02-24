---
title: "GLSL基础函数"
date: 2024-04-05T21:23:52+00:00
# weight: 1
# aliases: ["/first"]
tags: ["教程","CV"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
math: false
hidemeta: false
comments: false
description: "GLSL基础函数分类整理表格"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
---

以下是GLSL常用函数的分类整理表格：

| **函数类别**       | **函数名**                | **功能描述**                                                                | **示例/备注**                                                      |
|--------------------|---------------------------|-----------------------------------------------------------------------------|--------------------------------------------------------------------|
| **数学运算**       | `sin(x)/cos(x)/tan(x)`    | 三角函数（参数为弧度）                                                      | `sin(radians(30.0))` 计算30度角的正弦值                            |
|                    | `pow(x,y)`                | 计算x的y次幂                                                                | `pow(2.0,3.0)=8.0`   	                                            |
|                    | `sqrt(x)/inversesqrt(x)`  | 平方根 / 平方根倒数                                                         | `sqrt(4.0)=2.0`，`inversesqrt(4.0)=0.5`                            |
|                    | `exp(x)/log(x)`           | 以e为底的指数函数 / 自然对数                                                | `exp(1.0)=e≈2.718`，`log(e)=1.0`                                   |
|                    | `mod(x, y)`               | 取模运算 (`x - y * floor(x/y)`)，用于周期性循环。                           | `mod(7.5, 3.0) = 1.5`                                              |
| **几何操作**       | `dot(a,b)/cross(a,b)`     | 向量点积（用于光照计算） / 向量叉积（仅三维向量）                           | `dot(vec3(1,0,0),vec3(0,1,0))=0.0`                                 |
|                    | `length(v)/distance(a,b)` | 向量长度 / 两点间距离                                                        | `length(vec3(0,3,4))=5.0`，`distance(vec2(0,0),vec2(3,4))=5.0`    |
|                    | `normalize(v)`            | 单位化向量                                                                  | `normalize(vec3(0,3,4))=vec3(0,0.6,0.8)`                           |
| **纹理采样**       | `texture2D(sampler,uv)`   | 2D纹理采样（返回`vec4`颜色）                                                | `texture2D(tex,uv).rgb`获取纹理颜色                                |
|                    | `textureCube(sampler,dir)`| 立方体贴图采样                                                              | 用于天空盒或环境贴图                                               |
| **比较与插值**     | `step(edge,x)`            | 若`x≥edge`返回1.0，否则返回0.0                                              | `step(0.5,0.3)=0.0`                                                |
|                    | `smoothstep(edge0,edge1,x)` | 在`edge0`到`edge1`之间进行平滑插值（S形曲线）                              | 用于抗锯齿或渐变过渡                                              |
|                    | `clamp(x,min,max)`        | 将x限制在[min,max]范围内                                                    | `clamp(1.5,0.0,1.0)=1.0`                                           |
|                    | `mix(x,y,a)`              | 线性插值：`x*(1−a) + y*a`                                                   | `mix(0.0,2.0,0.5)=1.0`                                             |
| **矩阵运算**       | `matrixCompMult(a,b)`     | 矩阵逐分量相乘（非矩阵乘法）                                                | 结果矩阵的每个元素为`a[i][j] * b[i][j]`                            |
|                    | `transpose(m)/inverse(m)` | 矩阵转置 / 矩阵求逆（需方阵）                                               | `transpose(mat2(1,2,3,4))=mat2(1,3,2,4)`                           |
| **随机与噪声**     | `fract(x)`                | 获取小数部分（常用于伪随机数生成）                                          | `fract(2.3)=0.3`                                                   |
|                    | `noise(x)`                | Perlin噪声生成（部分版本支持）                                              | 需注意版本兼容性                                                   |


### 补充说明：
1. **函数分类依据**：根据功能分为数学、几何、纹理等类别，避免逐条列举。
2. **矩阵存储规则**：GLSL矩阵为列主序（如`mat2`构造时按列填充）。

完整函数列表可参考 {{<newtabref href="https://docs.gl" title="GLSL文档">}}。
