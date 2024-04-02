---
title: 实验进程
author: 2c984r83y
date: 2024-04-01 23:33:33 +0800
categories: [Blogging]
tags: [HelloWorld]
pin: false
---
**涨点!**
![20240402214836](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214836.png)

## Dataset

DSEC 数据集无法在机械硬盘上快速读取，dataloader会卡住。一次性读取15张png也会卡住。

disp应当使用int16的，而不是除以256后的int8类型，这会影响精度。

1. 重构为单通道图像
   26343张，99:1
   ![20240402214659](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214659.png)
2. 重构为三通道图像
   26343张，99:1
   ![20240402214726](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214726.png)
3. 重构为三通道归一化图像
   25735张，9:1
   ![20240402214747](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214747.png)

## Baseline

1.BGNet+
1.1 BGNet+PNG
![20240402215525](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402215525.png)
![20240402215435](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402215435.png)
1.2 BGNet+
2.ACVNet

3.Fast-ACVNet