+++
title = '线性子空间、行空间、零空间'
date = 2024-07-05T22:13:12+08:00
draft = false
hidden = true
tags = ['Mathematics', 'Notes']
isCJKLanguage = true
+++

[线性代数目录](../)

# 线性子空间

- **定义**：在 $n$ 维向量空间 $\mathbb F^n$ 中，一个线性子空间 $\mathcal N$ 是向量空间的一个非空子集，且必须满足以下两条性质：
  - 对于任意两个向量 $\vec v,\vec u\in\mathcal N$，都有 $\vec v+\vec u\in\mathcal N$；
  - 对于任意向量 $\vec v\in\mathcal N$ 和标量（scalar） $k\in\mathbb F$，都有 $k\vec v\in\mathcal N$。
- 仅包含零向量的集合 $\set{\vec 0}$ 和整个向量空间 $\mathbb F^n$ 也是线性子空间。这两者称为向量空间 $\mathbb F^n$ 的**平凡子空间**。
- 零向量 $\vec 0$ 是向量空间内所有子空间的元素，因为子空间必定是非空的，所以有向量 $\vec v$，从而 $\vec 0=0\cdot\vec v$ 也一定在子空间内。
- **定理**：对于任何有限维度向量空间 $\mathbb F^n$ 内的子空间 $\mathcal N$ 而言，都存在一组有限个向量 $\vec v_1,\vec v_2,\ldots,\vec v_n$，使得 $\mathcal N$ 中的任意元素 $\vec w$ 都能被写做 $c_1\vec v_1+c_2\vec v_2+\ldots+c_n\vec v_n$ 的形式
  - 如果向量组 $\vec v_1,\vec v_2,\ldots,\vec v_n$ 是线性无关的，则它们称为该子空间的一组[基](../basics)。
  - 在这种情况下，我们称该子空间的维数为 $n$，记作 $\operatorname{dim}\mathcal N=n$。
    - 仅包含零向量的子空间 $\mathcal M=\set{\vec 0}$ 的维数定义为 $0$。

## 矩阵子空间：列空间与零空间

