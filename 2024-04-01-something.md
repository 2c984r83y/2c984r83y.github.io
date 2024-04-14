---
title: Something
author: 2c984r83y
date: 2024-04-01 23:33:33 +0800
categories: [Blogging]
tags: [HelloWorld]
pin: false
---
**为什么不涨点？!**  
![20240402214836](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214836.png)
**涨了!**
![20240414173245](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240414173245.png)

## SECFF

将10通道的事件不断减少1/2的数量, 直到10层, 然后放入ResNet中

## Dataset

> disp 应当使用 int16 的，而不是除以 256 后的 int8 类型，这会影响精度。  

### DSEC

数据集激光雷达 Ground Truth 为 10 Hz, 数据集 Dataloader 默认取每个 Ground Truth 前 50ms 的事件.
> 50ms 与 100ms 会影响精度吗?  

DSEC 数据集无法在机械硬盘上快速读取，dataloader会卡住。一次性读取15张png也会卡住。

1. 重构为单通道图像(26343张，99:1)
   15个通道累加为一个通道，
   
   ![20240402214659](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214659.png)
2. 重构为三通道图像(26343张，9:1)
   15个通道压缩为三个
   
   ![20240402214726](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214726.png)
3. 重构为三通道归一化图像(26343张，9:1)
   15个通道压缩为三个并归一化
   > 这样会更好吗？
   > 不会
   25735张，9:1
   ![20240402214747](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402214747.png)
4. 重构为三通道中值图像(26343张，9:1)
   
## Baseline

### BGNet_Plus

#### BGNet+PNG

1px:17.16%

`/disk2/users/M22_zhaoqinghao/BGNet/logs_dsec_png`

![20240402215435](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240402215435.png)

```python
parser.add_argument('--batch_size', type=int, default=32, help='training batch size')
parser.add_argument('--test_batch_size', type=int, default=16, help='testing batch size')
parser.add_argument('--epochs', type=int, default=400, help='number of epochs to train')
parser.add_argument('--lr', type=float, default=0.001, help='base learning rate')
parser.add_argument('--lrepochs',default="100,200,220,300:10", type=str,  help='the epochs to decay lr: the downscale rate')
```

1.2 BGNet+Coordattention+Png

2.ACVNet

3.Fast-ACVNet

