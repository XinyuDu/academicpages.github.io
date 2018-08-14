---
title: "通过最小化损失函数训练神经网络"
collection: teaching
type: "人工神经网络教程-2"
permalink: /teaching/2018-NN-2
venue: "杜新宇,中科院北京纳米能源与系统研究所"
date: 2018-08-14
location: "中国, 北京"
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# 1. 损失函数

当我们构建了一个神经网络后，其最初的权重$$w_k$$和偏移量$$b_l$$都是随机的。这种神经网络并不能正确执行任务。我们还需要对其进行训练，使其在给定输入数据$$x$$时的输出$$o$$与$$x$$的标注值$$L(x)$$尽量相近，这种用于训练神经网络的数据集称为训练数据集。例如，我们输入数据$$x$$为一张猫的图片,则$$L(x)=cat$$，我们期望输出$$o=cat$$而不是$$o=dog$$。由于神经网络的输出值是向量，因此我们希望$$L(x)和o$$这两个向量之差的几何距离$$\parallel L(x)-o\parallel$$越短越好，几何距离越短神经网络的实际输出和我们期望的输出越相近。而且我们希望神经网络在训练数据集的所有训练数据上都实现短的几何距离，即要求$$\sum_{i=1}^n \parallel L(x)-o\parallel$$尽量小。我们可以将权重和偏移量当做自变量构建一个函数，来用前面介绍的梯度下降法来获得最小值，这个函数称为损失函数(cost function)：
$$
C(w,b)=\frac{1}{2n}\sum_{i=1}^n\parallel L(x)-o \parallel ^2 \tag{2-1}
$$
其中，$$w$$代表网络中权重的集合，$$b$$偏移量的集合，$$n$$是训练数据集的个数，$$o$$是输出向量，$$L(x)$$是标注值向量。这种形式的损失函数称为二次损失函数，其值肯定非负，当所有训练数据的输出值与标注值相等时，该损失函数值为0。



# 2.训练神经网络

训练神经网络本质上就是找到一组合适的权重和偏移量使得损失函数的值最小。可以利用前面讲过的梯度下降法来求其最小值。求解过程如下：

1. 首先我们有梯度向量$$\nabla C=(\frac{\partial f}{\partial w_k},\frac{\partial f}{\partial b_l})$$

2. 根据梯度向量更新$$w_k$$和$$b_l$$，直到取得损失函数$$C$$的最小值。

$$
w_k'=w_k-\eta \frac{\partial C}{\partial w_k}\tag{2-2}
$$

$$
b_l'=b_l-\eta \frac{\partial C}{\partial b_l}\tag{2-3}
$$

需要注意的是，式（2-1）损失函数包含求和操作，因此计算梯度向量时需要对所有训练数据进行偏微分操作再求和。当训练数据集的数量很大时计算量会非常大，导致训练速度缓慢。一个简单的解决办法是不对所有训练数据求和，而是随机的挑选小批量的训练数据进行计算，这种方法称为<b>随机梯度下降法</b>。

令$$C_x=\frac{\parallel L(x)-o \parallel}{2}^2$$，则$$C=\frac{1}{n}\sum_x C_x$$。梯度向量为：$$\nabla C=\frac{1}{n}\sum_x \nabla C_x$$

从训练数据集中随机选取m（m<n）条数据，使得：
$$
\frac{\sum_1^m\nabla C_x}{m}\approx\frac{\sum_1^n\nabla C_x}{n}=\nabla C \tag{2-4}
$$
因此，将上式带入（2-2）和（2-3）有：
$$
w_k'=w_k-\frac{\eta}{m}\sum_1^m\frac{\partial C_x}{\partial w_k}\tag{2-5}
$$

$$
b_l'=b_l-\frac{\eta}{m}\sum_1^m\frac{\partial C_x}{\partial b_l}\tag{2-6}
$$
