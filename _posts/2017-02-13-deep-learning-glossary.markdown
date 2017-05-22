---
layout: post
title: Deep learning - Glossary (Continuously updated...)
chineseTitle: 深度学习 - 术语 （持续更新...）
icon: microchip
date:   2017-02-17 10:50:57 +0800
categories:
  - deep-learning
tags:
  - glossary
---

---

## Overview

深度学习（Deep Learning, 简称DL）。

---

## Contents

- [**Neuro Network** - 神经网络](#neuro-network)
  - [**Layers** - 层](#layers)
  - [**MLPs** - 多层感知器](#mlps)
  - [**RNN** - 循环神经网络](#rnn)
    - [**LSTM** - Long Short Term 网络](#lstm)
  - [**CNN** - 卷积神经网络](#cnn)
    - [**Convaluatin and Pooling** - 卷积与池化](#cnn)
  - [**GAN** - 生成对抗神经网络](#gan)
- [**Activation Function** - 激活函数](#activation-function)
  - [**Sigmoid** - Sigmoid](#sigmoid)
  - [**Tanh** - Tanh](#tanh)
  - [**ReLU** - ReLU函数](#relu)
  - [**Other Activations** - 其他激励](#other-activation)
- [**Training** - 训练](#training)
  - [**Weights and Bias** - 权重和偏置](#weights-and-bias)
  - [**Softmax Regression** - Softmax回归](#softmax-regression)
  - [**Cross-Entropy** - 交叉熵](#cross-entropy)
  - [**Gradient Descent** - 梯度下降](#gradient-descent)
    - [**BGD** - 批量梯度下降法](#bgd)
    - [**SGD** - 随机梯度下降法](#sgd)
    - [**MBGD** - 小批量梯度下降法](#mbgd)
- [**NLP** - 神经语言程序学](#nlp)


---

## Neuro Network

### Layers

神经网络：输入层、输出层、隐藏层。

### MLPs

多层感知器，使用的是Sigmoid神经元，非感知器。


### RNN

Recurrent Neural Networks, 循环神经网络

#### LSTM

Long-Short Terms.

### CNN

卷积神经网络

#### Convaluatin and Pooling

卷积和池化（常用的max-pooling, average-pooling, L2-norm pooling)。

### GAN

生成式对抗神经网络

---

## Activation Function

### Sigmoid

Sigmoid 是常用的非线性的激活函数，它的数学形式如下：

![Sigmoid]({{ site.url }}/static/deep-learning-glossary/sigmoid.png)

### Tanh

Sigmoid的改良版，Tanh是0均值的，数学形式如下：

![Tanh]({{ site.url }}/static/deep-learning-glossary/tanh.png)

### ReLU

近年来，ReLU 变的越来越受欢迎。它的数学表达式如下：

![RelU]({{ site.url }}/static/deep-learning-glossary/relu.png)

如图可见，当输入 x <= 0时，输出恒为0，当输入 x > 0时，输出为本身。

![RelU-Effect-Picture]({{ site.url }}/static/deep-learning-glossary/relu-effect-picture.png)

> 优点：相比于 sigmoid/tanh，ReLU 只需要一个阈值就可以得到激活值，而不用去算一大堆复杂的运算。

> 缺点：一个非常大的梯度流过一个 ReLU 神经元，更新过参数之后，这个神经元再也不会对任何数据有激活现象了。如果这个情况发生了，那么这个神经元的梯度就永远都会是0。

根据RelU，有一些类似改动的函数：Leaky-ReLU、P-ReLU、R-ReLU应用时在不同场景与

### Other Activations

![Activation-Functions-1]({{ site.url }}/static/deep-learning-glossary/activation-functions-1.png)
![Activation-Functions-2]({{ site.url }}/static/deep-learning-glossary/activation-functions-2.png)

---

## Training

### Weights and Bias

Weights(权重值)，Bias(偏置值)。

### Softmax Regression

Softmax回归（归一化）。

### Cross-Entropy

现有关于样本集的2个概率分布p和q，其中p为真实分布，q非真实分布。使用错误分布q来表示来自真实分布p的平均编码长度, 表示如下图：

![Cross-Entropy]({{ site.url }}/static/deep-learning-glossary/cross_entropy.png)

因为用q来编码的样本来自分布p，所以期望H(p,q)中概率是p(i)。H(p,q)我们称之为“交叉熵”。

### Gradient Descent

#### BGD

Batch Gradient Descent(批量梯度下降法)。

#### SGD

Stochastic Gradient Descent(随机梯度下降法)。

#### MBGD

Mini-batch Gradient Descent(小批量)。

---

## NLP

NLP是神经语言程序学(Neuro-Linguistic Programming)。
