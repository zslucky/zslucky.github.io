---
layout: post
title: Deep learning intro with ConvNetJS.
chineseTitle: ConvNetJS深度学习入门简介
icon: desktop
date: 2016-12-21 10:50:57 +0800
published: false
categories:
  - javascript
tags:
  - deep-learning
---

---

## Overview

**原文引自**：[ConvNetJS](http://cs.stanford.edu/people/karpathy/convnetjs/intro.html).

This article is a simple example for starter to kick off his deep learning travel.

## Contents

- [**Introduce** - 介绍](#introduce)

---

## Introduce

### Brief intro to Deep Learning: Digit Classification Example

---

Several large companies (Google, Facebook, Microsoft, Baidu) now use Deep Learning models for various Machine Learning tasks, most notably and successfully speech and image recognition, and slowly natural language processing. [Read more](https://www.wired.com/2013/12/facebook-yann-lecun-qa/).

很多大公司（Google, Facebook, Microsoft, Baidu）现在使用深度学习模型来完成各种各样机器学习的任务，多数可靠和成功的语音图像识别，稍慢的自然语言处理。

Lets use the special case of image classification as a working example. Deep Learning is about stacking different types of transformers (layers) on top of each other. Like when you make a sandwich. Unlike a sandwich however, each layer accepts a volume of numbers (we like to call them activations since we think of each number as a firing rate of a neuron) and transforms it into a different volume of numbers using some set of internal parameters (we like to think of those as trainable synapses).

让我们使用一个特殊的图像分类示例来作为一个例子。深度学习就像是将各种不同类型的变形器（层）相互堆叠在一起，就像你做一个三明治。然而并不是三明治，每一层都接受一个数字卷（我们通常乐意称之为激励，因为我们将每个数字看作是一个神经元的激发率）并使用一些内部参数集（我们通常乐意将它们看作是可训练的神经元）将其转换到另一个数字卷。

In the simplest and most traditional setup, the first volume of activations represents your input and the last volume represents probabilities of the input volume being among any one of several distinct classes. During training you provide the network many examples of pairs of (input volume, class) and the network tunes its parameters ("learns") to transform inputs into correct class probabilities.

在最简单也最传统的设置中，第一个激励卷展现了你的输入，最后一个卷展现了输入在多个唯一类型中可能的概率。在训练中，你提供很多例子给网络，网络会调整她的参数来转换输入信息到正确的类型的概率。

Here's an MNIST digits example: suppose we have an image (a 28x28 array of pixel values) that contains a 4. We create a 2D volume of size (28,28) and fill it with the pixel values. Then we pipe the input volume through the network and get output volume of 10 numbers representing the probability that the input is any one of 10 different digits:

这里有一个MNIST手写数字的例子：假设我们有个图片（）他是数字4，我们创建一个2D的28*28的卷并填充进去像素值，然后我们让它通过网络，便会得到一个展现输入数字为10个不同数字概率的卷。

So we transformed the original image into probabilities (taking many intermediate forms of representation along the way (that's the gray boxes in my artistic rendering). Every box is a 3-dimensional array of numbers). But wait, it looks like the network only assigned 10 percent probability to this input being a 4, and 60 percent to it being a 9! That's fine: by design the mapping from input to output is a mathematical function that is parameterized by some numbers and we can tune these parameters to make the network slightly more likely to give class 4 a higher probability for this particular input in the future.

The details require a bit of calculus, but you can write down the expression for the probability of digit 4 for this particular input (this is what we wish to increase) and differentiate it to derive the expression for the gradient with respect to all of network's parameters. The gradient is the vector in the parameter space along which your function (here probability of 4) increases the most. So we forwarded our image of 4, computed the probability of answer being 4, then computed the gradient on all parameters of the network, and finally we nudge all the parameters slightly along that gradient. Some people call this procedure backpropagation (or backprop), but in the form I presented it's also just stochastic gradient descent, a vanilla method in optimization literature.

The amount we nudge is called the learning rate, and is perhaps the single most important number in training these networks. If it's high, the networks learn faster, but if it's too high, the networks can explode. On the other hand, if it's too low the training will take a very long time. Usually you start it higher (for example say 0.1), but not too high (!) and anneal it slowly over time a few orders of magnitude (down to 0.0001 perhaps).

In any case, what it comes down to is that after the nudge the network will be a tiny bit more likely to predict a 4 on this image. So we just start from some random parameters, repeat this procedure for tens of thousands of different digits, and the network gradually transforms itself into a digit classifier.