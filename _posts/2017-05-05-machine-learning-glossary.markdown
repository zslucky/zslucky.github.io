---
layout: math-post
title: Machine learning - Glossary (Continuously updated...)
chineseTitle: 机器学习 - 术语 （持续更新...）
icon: microchip
date:   2017-02-17 10:50:57 +0800
categories:
  - machine-learning
tags:
  - glossary
---

---

## Overview

Machine Learning, 简称ML。

---

## Contents

- [**Concerptions** - 概念](#concerptions)
  - [**One-Hot Encoding** - 独热编码](#one-hot-encoding)
  - [**Bias and variance** - 偏差和方差](#bias-and-variance)
    - [**RMSE** 均方根误差](#rmse)
  - [**Gini impurity and Entropy** Gini不纯度和熵](#gini-impurity-and-entropy)
  - [**Information Gain** 信息增益](#information-gain)
- [**Machine Learning Type** - 机器学习类型](#machine-learning-type)
  - [**Supervised Learning** - 监督学习](#supervised-learning)
  - [**Unsupervised Learning** - 非监督学习](#unsupervised-learning)
  - [**Reinforcement Learning** - 强化学习](#reinforcement-learning)
  - [**Transfer Learning** - 迁移学习](#transfer-learning)
- [**Data Cleaning** - 数据清洗](#data-cleaning)
  - [**Outier** - 异常值](#outlier)
  - [**NaN** - 缺失值](#nan)
    - [**MissForest** - MissForest缺失值填补法](#missforest)
    - [**MICE** - MICE](#mice)
- [**Feature Engineering** - 特征工程](#algorithms)
  - [**PCA** - 主成分分析](#pca)
  - [**ICA** - 独立成分分析](#ica)
- [**Algorithms** - 算法](#algorithms)
  - [**Naive Bayes** - 朴素贝叶斯](#naive-bayes)
  - [**SVM** - 支持向量机](#svm)
  - [**Decision Tree** - 决策树](#decision-tree)
  - [**KNN** - K邻近算法](#knn)
  - [**Adaboost** - Adaboost](#adaboost)
  - [**Random Forest** - 随机森林](#random-forest)
  - [**K-means** - K均值](#k-means)
  - [**t-SNE** - t分布邻域嵌入](#t-sne)
  - [**Gradient Boosting** - 梯度提升](#gradient-boosting)
- [**Ensemble** - 融合](#ensemble)
  - [**Bagging** - Bagging](#bagging)
  - [**Boosting** - Boosting](#boosting)
  - [**Stacking** - Stacking](#stacking)
  - [**Blending** - Blending](#blending)

---

## Concerptions

### One-Hot Encoding (1-hot)

独热编码即 One-Hot 编码，又称一位有效编码，其方法是使用N位状态寄存器来对N个状态进行编码，每个状态都由他独立的寄存器位，并且在任意时候，其中只有一位有效。

### Bias and variance

**Bias(偏差)**

**Variance(方差)**

#### RMSE

均方根误差，同样的还有RMLSE(对数均方根误差)、RMS(均方根值)、标准差(Standard Deviation)

### Gini impurity and Entropy

基尼不纯度(Gini impurity)

熵(Entropy)，又称香浓熵

### Information Gain

信息增益(Information Gain)

---

## Machine Learning Type

### Supervised Learning

有监督的学习，通常函数形式为$$ y=f(x) $$, 用已知的一系列x值及其对应的结果y值来进行训练得到函数$$ f() $$.

### Unsupervised Learning

无监督的学习，通常函数形式为$$ f(x) $$, 常用于聚类等，用已知的一系列x值来进行训练得到函数$$$ f()$$

### Reinforcement Learning

强化学习

### Transfer Learning

迁移学习

---

## Data Cleaning

### Outlier

异常值

### NaN

#### MissForest

MissForest缺失值处理方法

#### MICE

MICE缺失值处理方法

---

## Feature Engineering

### PCA

PCA(Principal Component Analysis)主成分分析

### ICA

ICA(Independent Component Correlation Algorithm)独立成分分析

---

## Algorithms

### Naive Bayes

朴素贝叶斯算法，常用的如高斯朴素贝叶斯算法。

### SVM

支持向量机算法，可提供多种核函数，如线性核函数(Linear)、高斯核函数(RBF)、多项式核函数和sigmoid核函数。

### Decision Tree

决策树算法

### KNN

K邻近算法

### Adaboost

Adaboost算法

### Random Forest

随机森林算法

### K-means

K均值算法

### t-SNE

t分布邻域嵌入算法

### Gradient Boosting

梯度提升算法, GBDT(梯度提升决策树)、GBDRT(梯度提升回归树)、GBM

---

## Ensemble

### Bagging

### Boosting

### Stacking

### Blending
