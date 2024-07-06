+++
title = '特征值、特征向量、特征多项式'
date = 2024-07-05T21:55:12+08:00
draft = false
hidden = true
tags = ['Mathematics', 'Notes']
isCJKLanguage = true
+++

[线性代数目录](../)

# 特征值、特征向量、特征多项式

## 特征值与特征向量

- **定义**：对于矩阵 $A$，若*存在*常数 $k$ 和*非零*向量 $\vec v$，满足 $A\vec v=k\vec v$，则称 $k$ 为 $A$ 的特征值之一，$v$ 为 $A$ 关于 $k$ 的特征向量。
  - 例如，$\begin{pmatrix} 0 & 1 \cr 2 & 1 \end{pmatrix} \begin{pmatrix} 2 \cr 4 \end{pmatrix} = \begin{pmatrix} 4 \cr 8 \end{pmatrix} = 2 \begin{pmatrix} 2 \cr 4 \end{pmatrix}$，因此矩阵 $\begin{pmatrix} 0 & 1 \cr 2 & 1 \end{pmatrix}$ 有特征值 2，其对应的特征向量为 $\begin{pmatrix} 2 \cr 4 \end{pmatrix}$（或者它的任意非零倍数）。
- **定理**：方阵可逆，当且仅当它没有零特征值。
  - **证明**：若方阵 $A$ 拥有特征值 $0$，则一定存在非零向量 $\vec v$ 满足 $A\vec v=\vec 0$，故矩阵 $A$ 不可逆。
  - 反之，若方阵 $A$ 没有零特征值，则其零空间的维数 $N()