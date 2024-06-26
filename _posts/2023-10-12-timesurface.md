---
title: 事件相机图像重构：浅谈Time-Surface
author: 2c984r83y
date: 2023-10-12 23:33:33 +0800
categories: [TecDoc, Paper Reading]
tags: [Event-based vision, Time-surface]
pin: false
math: true
mermaid: true
---
## 事件相机简介

事件相机只输出亮度变化超过一定阈值的像素坐标、时间戳与极性。因为事件流是异步产生的，与传统的固定事件曝光的图像相比富含更时空信息，所以将事件流转换为图像帧的算法至关重要，选对了算法就能发挥事件相机的高时间分辨率优势。事件相机与传统的高速相机相比，输出的事件流中不含冗余的背景图像信息，事件流只输出亮度变化超过阈值的运动物体的信息，这是我们感兴趣的，因此提高了处理速度，降低了数据量，避免使用过多的硬件资源，更利于实时计算。

事件相机的一般输出形式是事件流，事件流可以表示成如下的形式：

$ev_i=[\mathbf{x_i},t_i,p_i]^T,\quad i\in\mathbb{N},$

$ev_i$是第i个事件，其坐标为$\mathbf{x_i}=[x_i,y_i]^T$

时间戳$t_i$，极性$ p_{i}\in\{-1,1\}$，-1表示OFF事件，1表示ON事件

像素异步生成事件，形成时空点云，代表对象的空间分布和动态行为。
![20231013165452](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231013165452.png)

## Time-Surface[1]

Time-Surface 是一种重构事件流的方法，它将事件流转换为图像帧，使得事件流的信息可以用传统的计算机视觉算法处理. Time-Surface 的核心思想是将事件流的时间信息转换为空间信息，将事件流的时间戳$t_i$转换为像素的灰度，这样就可以将事件流转换为图像帧。[1]

以 $\mathbf{x_i}=[x_i,y_i]^T$ 为中心的一个 $2R+1\times2R+1$ 的窗口:
可以表示为: $ T_i(\mathbf{u},p)=\max_{j\leq i}\{t_j|\mathbf{x_j}=(\mathbf{x_i}+\mathbf{u}),p_j=p\} $

其中 $T_i(\mathbf{u},p)$ 是 time-context，时间上下文

$\mathbf{u}=\begin{bmatrix}u_x,u_y\end{bmatrix}^T$ 是窗口中的像素, $u_x\in\{-R,\ldots,R\}$ $u_y\in\{-R,\ldots,R\}$

Time-Surface 中的每个像素的灰度值度是时间值的编码：亮像素显示最近的活动，而暗像素接收过去更远的事件（为了清楚起见，图中仅表示与 OFF 事件相对应的时间值）
![20231013212413](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231013212413.png)
$\mathcal{S}_i(\mathbf{u},p)$ 是 $ev_i$ 附近的的 time-surface:
$\mathcal{S}_i(\mathbf{u},p)=e^{-(t_i-\mathcal{T}_i(\mathbf{u},p))/\tau}$
Time-Surface 提供了动态的时空上下文信息,指数衰减系数扩展了时间邻域中的信息
![20231013213421](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231013213421.png)
三维视角下的 Time-Surface:
![20231013213439](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231013213439.png)

### Time-Surface Prototypes

为了构建整个平面而不是窗口的局部 Time-Surface, 这里使用 incremental clustering process (增量聚类)生成 Time-Surface.
![20231016100619](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231016100619.png)

> [1] LAGORCE X, ORCHARD G, GALLUPPI F, 等. HOTS: A Hierarchy of Event-Based Time-Surfaces for Pattern Recognition[J/OL]. IEEE Transactions on Pattern Analysis and Machine Intelligence, 2017, 39(7): 1346-1359. DOI:[10.1109/TPAMI.2016.2574707](https://doi.org/10.1109/TPAMI.2016.2574707).

## 速度不变的 Time-Surface[2]

A common representation used in event-based vision is the Surface of Active Events [7], also referred as Time Surface [30]. The Time Surface T at a pixel (x, y) and polarity p is defined as T (x, y, p) ← t, (2) where t is the time of the last event with polarity p occurred at pixel (x, y).

Time-Surface 的局部可能有很大的变化。事实上，根据速度、方向和拐角的对比度，Time-Surface 的可能会有很大变化。为了保持分类模型的紧凑和高效，对其输入引入一些归一化非常重要。

为了角点检测角点,应当使用相对时间而不是绝对的时间戳.然而在每个事件发生时更新一次局部的 Time-Surface 太过于昂贵,而且存储每个像素的多个时间戳,消耗资源.
![20231017205957](https://raw.githubusercontent.com/2c984r83y/2c984r83y.github.io/master/images/20231017205957.png)

`若周围更高,则把S(x,y,p)周围的都削一圈,把S(x,y,p)赋值为(2r+1)^2(那就是把(2r+1)^2减1?)`

> [2]MANDERSCHEID J, SIRONI A, BOURDIS N. Speed Invariant Time Surface for Learning to Detect Corner Points With Event-Based Cameras[C/OL]//2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR). Long Beach, CA, USA: IEEE, 2019: 10237-10246[2023-10-10]. [https://ieeexplore.ieee.org/document/8954376/](https://ieeexplore.ieee.org/document/8954376/). DOI:[10.1109/CVPR.2019.01049](https://doi.org/10.1109/CVPR.2019.01049).
