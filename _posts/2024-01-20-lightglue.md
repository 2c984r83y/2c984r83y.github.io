---
title: Light Glue
author: 2c984r83y
date: 2023-12-04 21:30:00 +0800
categories: [TecDoc]
tags: [LightGlue]
pin: false
math: true
mermaid: true
---
## [Superglue](https://github.com/magicleap/SuperGluePretrainedNetwork)

>Superglue training code 并没有开源，只开源了 pretrained model

### 1. 安装

#### 1.1 安装依赖

服务器已经安装好以下依赖：

* Python 3 >= 3.5
* PyTorch >= 1.1
* OpenCV >= 3.4 (4.1.2.30 recommended for best GUI keyboard interaction, see this note)
* Matplotlib >= 3.1
* NumPy >= 1.18
本地安装依赖请参考以下命令：

```bash
pip3 install numpy opencv-python torch matplotlib
```

#### 1.2 clone 仓库到本地

```bash
git clone https://github.com/magicleap/SuperGluePretrainedNetwork.git
```

### 2.运行 demo

#### 2.1 Run the demo on a directory of images

对于给定的图像目录，运行以下命令，superglue 将读取文件目录中的两对图片进行匹配：

```bash
cd SuperGluePretrainedNetwork/
./demo_superglue.py --input assets/freiburg_sequence/ --output_dir dump_demo_sequence --resize 320 240 --no_display
```

![20240109211554](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240109211554.png)
运行结果：
![matches_000000_000011](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/matches_000000_000011.png)
_matches_000000_000011.png_

### 3. 运行 eval

对 supergleue 结果进行评估

```bash
./match_pairs.py --eval
```

![20240109212121](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240109212121.png)

```bash
Evaluation Results (mean over 15 pairs):
AUC@5    AUC@10  AUC@20  Prec    MScore
23.60    43.51   61.74   73.54   19.62
```

## [Lightglue](https://github.com/cvg/LightGlue)

### 1. 安装

#### 1.1 clone 仓库到本地

```bash
git clone https://github.com/cvg/LightGlue.git
```

#### 1.2 安装依赖

```bash
cd LightGlue
python -m pip install -e .
```

### 2. 运行 demo

启动 jypyter notebook
![20240109221648](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240109221648.png)
![20240109221714](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240109221714.png)
![20240109221736](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240109221736.png)

### 3. 运行 Benchmark

```bash
python benchmark.py
```

```bash
easy                               256     512    1024    2048    4096
----------------------------------------------------------------------
LightGlue-full                    20.3    20.3    20.7    43.9   141.1
LightGlue-adaptive                18.5    13.8    11.5    20.0    57.4

difficult                          256     512    1024    2048    4096
----------------------------------------------------------------------
LightGlue-full                    20.2    20.3    20.5    44.4   142.9
LightGlue-adaptive                13.7    18.5    18.7    27.4    66.9
```

## [Glue-Factory](https://github.com/cvg/glue-factory)

对 Lightglue 进行训练

### clone 仓库到本地

```bash
git clone https://github.com/cvg/glue-factory
```

### 安装依赖

```bash
cd glue-factory
python3 -m pip install -e .  # editable mode
python3 -m pip install -e .[extra]
```

### Evaluation

To evaluate the pre-trained SuperPoint+LightGlue model on HPatches, run:

```bash
python3 -m gluefactory.eval.hpatches --conf superpoint+lightglue-official --overwrite
```

### Debug

更新 scikit-learn 到最新，否则会报错

```bash
"/disk2/users/.local/lib/python3.8/site-packages/sklearn/externals/joblib/externals/cloudpickle/cloudpickle.py", line 148, in _make_cell_set_template_code return types.CodeType( TypeError: an integer is required (got type bytes)
```

```bash
pip3 install --upgrade scikit-learn
```

### Pre-train

Train LightGlue with SuperPoint. First pre-train LightGlue on the homography dataset

修改`gluefactory/configs/superpoint+lightglue_homography.yaml`，batch_size 改为32
>The default batch size of 128 corresponds to the results reported in the paper and requires 2x 3090 GPUs with 24GB of VRAM each as well as PyTorch >= 2.0 (FlashAttention)  

![20240112215310](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240112215310.png)

下载数据集耗时 42 Hours  
32 batch size 消耗显存9.2G VRAM
一个 epoch 耗时 1 Hour

```bash
python -m gluefactory.train sp+lg_homography --conf gluefactory/configs/superpoint+lightglue_homography.yaml
```

![20240114164518](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240114164518.png)
![20240118122806](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240118122806.png)
![20240118122526](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240118122526.png)
![20240118122547](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240118122547.png)
![20240118122605](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240118122605.png)
![20240118122618](https://raw.githubusercontent.com/2c984r83y/picgo_picbed/main/blog_img/20240118122618.png)