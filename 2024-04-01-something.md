---
title: Something
author: 2c984r83y
date: 2024-04-01 23:33:33 +0800
categories: [Blogging]
tags: [HelloWorld]
pin: false
---

![20240414173245](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240414173245.png)

## SECFF

将10通道的事件不断减少1/2的数量, 直到10层, 然后放入ResNet中
![20240402214836](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214836.png)

## Dataset

### DSEC

数据集激光雷达 Ground Truth 为 10 Hz, 数据集 Dataloader 默认取每个 Ground Truth 前 50ms 的事件.构建为voxel, 默认维度为 15*640*480.

> 50ms 与 100ms 会影响精度吗?

DSEC 数据集无法在机械硬盘上快速读取，dataloader会卡住。一次性读取15张png也会卡住。

#### disp(GroundTruth)

视差 disparity 真值 GroundTruth 图像来自于激光雷达, 10FPS.  

> disp 使用时需要除以256,使用浮点类型确保精度

![20240415152212](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240415152212.png)

#### png_1c

   重构为单通道图像 (26343张，99:1)
   15个通道累加为一个通道，忽略极性，灰度归一化到0-255

   ![20240414235819](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/000001.png)

#### png_3c

   重构为三通道图像(25735张，9:1)
   15个通道压缩为三个通道，忽略极性

   ![20240402214726](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214726.png)

#### png_3c_n

 重构为三通道归一化图像(25735张，9:1)
 15个通道压缩为三个通道，忽略极性，灰度归一化到0-255  
 与 png_3c 指标几乎一致  
   ![20240402214747](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214747.png)

#### *png_3c_m*

   重构为三通道中值图像(25735张，9:1)
   15个通道压缩为三个通道，不忽略极性，没有事件的地方灰度值为 128，负极性减少灰度值，正极性增加灰度值，灰度值归一化到0-255
   ![20240414235713](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240414235713.png)

## Baseline

### Fast-ACVNet

指标最佳,轻量化,考虑作为 baseline
![20240414173245](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240414173245.png)

### ACVNet

指标不太好,网络结构重,有待优化
![20240415151733](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240415151733.png)

### BGNet

指标不理想,暂不考虑作为baseline

![20240415151340](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240415151340.png)
