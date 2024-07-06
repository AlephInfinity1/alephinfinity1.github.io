+++
title = '线性代数基础'
date = 2024-07-01T10:23:23+08:00
draft = false
hidden = true
tags = ['Mathematics', 'Notes']
isCJKLanguage = true
+++

[线性代数目录](../_catalog)

# 何谓线性代数？
- 数学的一个分支，旨在研究线性关系、线性对象以及线性变换。
- 将现实生活中的现象通过矩阵和向量来建模，有助于更好地计算它们。
- 广泛的应用与诸多自然和社会科学中。在 AI 建模和计算中，应用面也十分广泛。
  - 在其他的数学分支（微积分、随机过程、概率模型）中也有所使用

# 一些基础定义
- **线性变换**（linear map）：一个线性变换函数 $f$ 一定满足：
  - $f(a)+f(b) = f(a+b)$
  - $kf(a) = f(ka)$
- **向量**（vector）：一系列数字的组合。例如 $\begin{pmatrix} 1 \cr 2 \cr 3 \end{pmatrix}$ 就是一个（列）向量。
  - 行向量也存在，但是不那么常用。它们可以写作 $\begin{pmatrix} 1 & 2 & 3 \end{pmatrix}$ 或者 $\begin{pmatrix} 1 \cr 2 \cr 3 \end{pmatrix}^T$。
- **矩阵**（matrix）：一个二维存储数字的线性对象。可以想成是一系列向量的集合。例如 $\begin{pmatrix} 1 & 0 \cr 0 & 1 \end{pmatrix}$ 是二维的单位矩阵。
  - 当我们提到一个矩阵是 $m\times n$ 的时候，该矩阵有 $m$ 行和 $n$ 列。
  - 若 $m=n$，则我们称该矩阵为**方阵**（$n$ 阶方阵）
    - 注意：**“阶”** 跟 **“秩”**（接下来会讲到）不一样！

## 向量和矩阵的运算
向量有以下两种基础运算：
- **加法**：将两个长度相同的向量，按照元素测序，一一加在一起
  - **例**：$\begin{pmatrix}1\cr 0\cr 2\end{pmatrix}+\begin{pmatrix}-3\cr 1\cr -1\end{pmatrix}=\begin{pmatrix}1-3\cr 0+1\cr 2-1\end{pmatrix}=\begin{pmatrix}-2\cr 1\cr 1\end{pmatrix}$
- **数乘**：向量 $\vec v$ 和标量 $k$ 之间相乘，将向量 $\vec v$ 中所有的值都乘以标量 $k$，得到一个新的向量 $k\vec v$
  - **例**：$2\cdot\begin{pmatrix}1\cr 0\cr -3\end{pmatrix} = \begin{pmatrix}2\cdot 1\cr 2\cdot 0\cr 2\cdot -3\end{pmatrix} = \begin{pmatrix}2\cr 0\cr -6\end{pmatrix}$。
- **减法**：$\vec a-\vec b=\vec a+(-1)\cdot\vec b$。

注意向量之间不存在乘法。

与此同时，矩阵有以下几种运算。在运算的时候，总是可以将矩阵视为一系列列向量的集合：
- **加法**：对于两个 $m\times n$ 的矩阵，可以将对应的元素加在一起，得到矩阵的和。
  - **例**：$\begin{pmatrix}1&a\cr 0&7\end{pmatrix}+\begin{pmatrix}3&4\cr -1&b\end{pmatrix}=\begin{pmatrix}4&a+4\cr -1&b+7\end{pmatrix}$
- **数乘**：与向量数乘相似，把所有元素与一个标量相乘，得到一个新的矩阵
  - **例**：$(-2)\cdot\begin{pmatrix} 2&3\cr -1&4\end{pmatrix} = \begin{pmatrix}-4&-6\cr 2&-8\end{pmatrix}$
- **与向量的乘法**：一个 $m\times n$ 的矩阵和一个 $n$ 阶列向量的乘积，是一个 $m$ 阶的列向量。
  - 可以把 $m\times n$ 矩阵视作 $n$ 个 $m$ 阶向量的集合：$A=\begin{pmatrix}\vec a_1&\vec a_2&\ldots&\vec a_n \end{pmatrix}$
  - 这样，可以定义 $A\vec x=A\begin{pmatrix}x_1\cr x_2\cr\vdots\cr x_n\end{pmatrix}=x_1\vec a_1+x_2\vec a_2+\ldots+x_n\vec a_n$
  - **例**：置换矩阵 $\begin{pmatrix}0&1&0\cr 0&0&1\cr 1&0&0\end{pmatrix}\begin{pmatrix}1\cr 2\cr 3\end{pmatrix} = 1\cdot\begin{pmatrix}0\cr 0\cr 1\end{pmatrix} + 2\cdot\begin{pmatrix}1\cr 0\cr 0\end{pmatrix} + 3\cdot\begin{pmatrix}0\cr 1\cr 0\end{pmatrix} = \begin{pmatrix}2\cr 3\cr 1\end{pmatrix}$
- **矩阵乘法**：一个 $m\times n$ 和一个 $n\times r$ 的矩阵的乘积，是一个 $m\times r$ 的矩阵。
  - 这里，一个拥有 $n$ 个元素的列向量可以视作一个 $n\times 1$ 的矩阵。
  - 设 $B=\begin{pmatrix}\vec b_1&\vec b_2&\ldots&\vec b_n \end{pmatrix}$，则 $AB=A\begin{pmatrix}\vec b_1&\vec b_2&\ldots&\vec b_n\end{pmatrix}=\begin{pmatrix}A\vec b_1&A\vec b_2&\ldots&A\vec b_n\end{pmatrix}$
  - 计算的时候，$AB$ 中处于位置 $(r,c)$ 的元素等于 $A$ 中第 $r$ 行所有元素与 $B$ 中第 $c$ 列所有元素的乘积的和。
  - **例**：$\begin{pmatrix}{\red 1}&{\red 2}\cr 3&4\end{pmatrix}\begin{pmatrix}{\blue 5}&6\cr {\blue 7}&8\end{pmatrix}=\begin{pmatrix}1\times 5+2\times 7&1\times 6+2\times 8\cr 3\times 5+4\times 7&3\times 6+4\times 8\end{pmatrix}=\begin{pmatrix}{\green{19}}&22\cr 43&50\end{pmatrix}$
    - $\green{19} = {\red 1}\times{\blue 5} + {\red 2}\times{\blue 7}$
- **转置**（transpose）：一个 $m\times n$ 的矩阵 $A$，它的转置 $A^T$ 是一个 $n\times m$ 的矩阵，其在位置 $(r,c)$ 的元素是原矩阵位于 $(c,r)$ 的元素。
  - **例**：$\begin{pmatrix}11&12\cr 21&22\cr 31&32\end{pmatrix}$ 的转置是 $\begin{pmatrix}11&21&31\cr 12&22&32\end{pmatrix}$，反之亦然
  - **定理**：任何矩阵 $A$ 的转置的转置 ${A^T}^T$ 都等于原矩阵本身
  - **定理**：$(AB)^T=B^TA^T$

## 矩阵作为线性变换的表示形式

对于实数数域 $\mathbb F=\mathbb R$ 和复数数域 $\mathbb F=\mathbb C$ 而言，所有 $m\times n$ 矩阵的集合（$\mathbb F^{m\times n}$）代表了所有 $\mathbb F^n\to\mathbb F^m$ 的线性变换的集合。
- 矩阵与线性变换是**同构**的。
- 矩阵与向量的乘法，表示线性变换函数的应用（application）。
- 矩阵之间的乘法则表示了线性变换函数的组合（composition）。
- 单位矩阵代表恒等函数（identity function），即 $f(\vec x)=\vec x$。
  - 以矩阵的形式，单位矩阵是对角线为 $1$，其余元素全部为 $0$ 的矩阵。例如 $\begin{pmatrix} 1&0&0\cr 0&1&0\cr 0&0&1 \end{pmatrix}$ 就是一个 3 阶单位矩阵。
  - 单位矩阵必定为方阵，因为只有当 $m=n$ 时，$\mathbb F^n\to\mathbb F^m$ 才有可能是恒等函数。
  - $n$ 阶的单位矩阵，一般记为 $I_n$。
- 方阵的逆代表了反函数。因为 $f^{-1}\circ f=id$，因此有 $A^{-1}A=I_n$
  - $AA^{-1}=A^{-1}A=I_n$
  - 就像不是所有函数都有反函数，也不是所有方矩阵都有逆矩阵。
  - 矩阵的可逆性会在后面讲到

## 线性有关性、基、秩

WIP