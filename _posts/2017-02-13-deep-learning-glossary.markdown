---
layout: post
title: Deep learning - Glossary (Continuously updated...)
chineseTitle: 深度学习 - 术语 （持续更新...）
icon: exchange
date:   2017-02-17 10:50:57 +0800
categories:
  - glossary
tags:
  - deep-learning
---

---

## Overview

深度学习（Deep Learning, 简称DL）。

---

## Contents

- [**Neuro Network** - 神经网络](#neuro-network)
  - [**Layers** - 层](#layers)
  - [**BP Neuro Network** - 前馈神经网络](#bp-neuro-network)
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
- [**Supervised Unsupervised and Reinforcement learning** - 有监督学习和无监督学习](#supervised-unsupervised-and-reinforcement-learning)
  - [**SVM** - 支持向量机](#svm)
  - [**KNN** - 邻近算法](#knn)
- [**Training** - 训练](#training)
  - [**Weights and Bias** - 权重和偏置](#weights-and-bias)
  - [**Softmax Regression** - Softmax回归](#softmax-regression)
  - [**Cross-Entropy** - 交叉熵](#cross-entropy)
  - [**Gradient Descent** - 梯度下降](#gradient-descent)
    - [**BGD** - 批量梯度下降法](#bgd)
    - [**SGD** - 随机梯度下降法](#sgd)
    - [**MBGD** - 小批量梯度下降法](#mbgd)
- [**Bayes** - 贝叶斯](#bayes)
- [**NLP** - 神经语言程序学](#nlp)
- [**One-Hot Encoding** - 独热编码](#one-hot-encoding)
---

## Neuro Network

### Layers

神经网络：输入层、输出层、隐藏层。

### BP Neuro Network

前馈神经网络

### RNN

循环神经网络

#### LSTM

Long-Short Terms.

### CNN

卷积神经网络

#### Convaluatin and Pooling

卷积和池化

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

## Supervised Unsupervised and Reinforcement learning

监督学习：每个数据点被标记或者与一个类别或者感兴趣值相关联。分类标签的一个例子是将图像指定为“猫”或者“狗”。价值标签的一个例子是销售价格与二手车相关联。监督学习的目标是研究许多这样的标记示例，进而能够对未来的数据点进行预测，例如，确定新的照片与正确的动物（分类（classification））或者指定其他二手车的准确销售价格（回归（regression））。

无监督学习：数据点没有标签对应。相反，一个无监督学习算法的目标是以一些方式组织数据或者表述它的结构。这意味着将其分组到集群内部，或者寻找不同的方式查看复杂数据，使其看起来更简单。

强化学习：对应于每一个数据点，算法需要去选择一个动作。这是一种常见的机器人方法，在一个时间点的传感器读数集合是一个数据点，算法必须选择机器人的下一个动作。这也是很普通的物联网应用模式，学习算法接收一个回报信号后不久，反馈这个决定到底好不好。基于此，算法修改其策略为了达到更高的回报。

![Supervised-Unsupervised-and-Reinforcement-learning]({{ site.url }}/static/deep-learning-glossary/supervised-unsupervised-and-reinforcement-learning.png)

### SVM
SVM(支持向量机)是一个有监督的学习模型，通常用来进行模式识别、分类以及回归分析。

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

## Bayes

贝叶斯分类算法是统计学的一种分类方法，它是一类利用概率统计知识进行分类的算法。在许多场合，朴素贝叶斯(Naïve Bayes，NB)分类算法可以与决策树和神经网络分类算法相媲美，该算法能运用到大型数据库中，而且方法简单、分类准确率高、速度快。

## NLP

NLP是神经语言程序学(Neuro-Linguistic Programming)。

## One-Hot Encoding (1-hot)

独热编码即 One-Hot 编码，又称一位有效编码，其方法是使用N位状态寄存器来对N个状态进行编码，每个状态都由他独立的寄存器位，并且在任意时候，其中只有一位有效。