---
layout: post
title:  "SVM 이론 정리"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: Dummy
tags: lorem ipsum
---



### 선형 SVM (Linear SVM)


![sds](https://drive.google.com/uc?id=1p8OsGhVG0gprim-7n7jRYF3WdTk7pl0w)

$${\mathcal{D} = \{ (\mathbf{x}_i, y_i)|\mathbf{x}_i \in \mathbb{R}^p, y_i \in \{-1,1\}\}_{i=1}^n}$$


$x_i = $ 실수
$y_i  =  \{-1 , 1\}$ 의 값을 가질때,

위의 예에서는  <span style="color:red">빨간색 공</span>은 -1, <span style="color:blue">파란색 공</span>은 +1 이다

<br>
#### 판별 함수

두 공은 나누는 초평면(or 직선)의 함수는
$${f(x) = w^Tx-w_0}$$


여기서 $w^T$는

<br>

두 벡터를 $\mathbf{w}\begin{pmatrix}-b\\-a\\1\end{pmatrix}$ 와  $\mathbf{x}\begin{pmatrix}1\\x\\y\end{pmatrix}$

$$\mathbf{w}^T\mathbf{x} = -b\times (1) + (-a)\times x + 1 \times y$$

$$\mathbf{w}^T\mathbf{x} = y - ax - b$$

예를 들면,

![ss](https://drive.google.com/uc?id=1lEwikbhT-WXZ4VOv0gVho3G0jHQca9gZ)
$w_0 = 0$ 으로 했을 때$$\mathbf{w}^T\mathbf{x}=0$$
$x_2 = -2x_1$ 이고

$\mathbf{w}\begin{pmatrix}2 \\1\end{pmatrix}$ 와 $\mathbf{x} \begin{pmatrix}x_1 \\ x_2\end{pmatrix}$  

$w$는 법선벡터가 된다.


<br>
### 서포트 벡터 (Support Vector)

${\mathbf{X}}^+$ : ${y_{i}=+1}$ 데이터중에서 초평면과 가장 가까운 데이터
${\mathbf{X}}^-$ : ${y_{i}=-1}$ 데이터중에서 초평면과 가장 가까운 데이터

${\mathbf{X}}^+$, ${\mathbf{X}}^-$를 <b>서포트 벡터</b>라 한다.



<br>

${\mathbf{X}}^+$, ${\mathbf{X}}^-$ 대입하게 되면

$w^Tx^+ - w_o > 0$ , $w^Tx^-+ - w_o < 0$


$$w^Tx^+ - w_o = 1 \text{ and } w^Tx^-+ - w_o \leq 1$$


$$w^Tx^+ - w_o \geq 1 \text{ and } w^Tx^-+ - w_o \leq 1$$
