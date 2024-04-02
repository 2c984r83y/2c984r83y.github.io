---
title: 实验进程
author: 2c984r83y
date: 2024-04-01 23:33:33 +0800
categories: [Blogging]
tags: [HelloWorld]
pin: false
---
**为什么不涨点？!**
![20240402214836](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214836.png)

## Dataset

DSEC 数据集无法在机械硬盘上快速读取，dataloader会卡住。一次性读取15张png也会卡住。

disp应当使用int16的，而不是除以256后的int8类型，这会影响精度。

1. 重构为单通道图像
   15个通道压缩为一个
   26343张，99:1
   ![20240402214659](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214659.png)
2. 重构为三通道图像
   15个通道压缩为三个
   26343张，99:1
   ![20240402214726](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214726.png)
3. 重构为三通道归一化图像
   15个通道压缩为三个并归一化
   > 这样会更好吗？  

   25735张，9:1
   ![20240402214747](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214747.png)

## Baseline

1.BGNet_Plus
1.1 BGNet+PNG

```python
parser.add_argument('--batch_size', type=int, default=32, help='training batch size')
parser.add_argument('--test_batch_size', type=int, default=16, help='testing batch size')
parser.add_argument('--epochs', type=int, default=400, help='number of epochs to train')
parser.add_argument('--lr', type=float, default=0.001, help='base learning rate')
parser.add_argument('--lrepochs',default="100,200,220,300:10", type=str,  help='the epochs to decay lr: the downscale rate')
```

`/disk2/users/M22_zhaoqinghao/BGNet/logs_dsec_png`  
1px:17.16%
![20240402215435](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402215435.png)

1.2 BGNet+Coordattention+Png


2.ACVNet

3.Fast-ACVNet