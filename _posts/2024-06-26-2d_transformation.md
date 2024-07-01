---

layout: single
title:  "2D Transformation" # 글 제목
categories: 
  - Geometry # category
tags: 
  - [] # tag
toc: true 
toc_sticky: true
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true 
use_math: true
typore-root-url: ../
---

**참고**

1. [다중관점기하학(Multiple View Geometry in Computer Vision) 책 내용 정리 Part 1 - 사영기하학](https://alida.tistory.com/13)

<br><br>

# 2D 변환 

![Screenshot from 2024-06-26 14-22-30](/images/2024-06-26-2dtransformation/Screenshot from 2024-06-26 14-22-30.png){:.img-display-100}

<center style="font-size:12px;">
    <a href=""> 출처 </a>
</center>



동차 좌표를 이용하여 표현하면 Affine 변환을 단일 행렬로 표현할 수 있고 이로 인해 여러 행렬의 변환들의 행렬곱으로 자유롭게 결합 할 수 있음

<br><br>

## 유클리디안  변환 (Euclidean transformation)

다른 말로는 **Rigid transformation** 

유클리디안 변환은 변환 후 물체의 크기와 형태가 유지되어야 하므로, 이동 변환과 회전 변환이 포함됨

<br>

### Translation

2차원 공간에서 위치를 이동하는 변환

아래와 같은 동차 좌표계를 이용한 식으로 표현 가능

<br>


$$
\bar{x}^{'}=\begin{bmatrix} I &  t\\ 0^{T} &1 \end{bmatrix}\bar{x}=\begin{bmatrix} 1 &  0 & t_{x} \\ 0 & 1 & t_{y}\\ 0 & 0 & 1\end{bmatrix}\bar{x}
$$

<br>

이동 변환의 자유도는 2

- $t_{x}$, $t_{y}$

<br>

### Rotation 

2차원 공간에서 회전하는 변환

반시계 반향으로 $\theta$ 만큼 회전시키는 행렬은 다음과 같음

<br>


$$
\bar{x}^{'}=\begin{bmatrix} R &  0\\ 0^{T} &1 \end{bmatrix}\bar{x}=\begin{bmatrix} \cos{\theta} &  -\sin{\theta} & 0  \\ \sin{\theta} & \cos{\theta} & 0\\ 0 & 0 & 1\end{bmatrix}\bar{x}
$$


회전 변환의 자유도는 1

- $\theta$

<br>

### Euclidean 

**Rotation + Translation**

유클리디안 거리와 각도가 보존됨


$$
\bar{x}^{'}=\begin{bmatrix} R &  t\\ 0^{T} &1 \end{bmatrix}\bar{x}=\begin{bmatrix} \cos{\theta} &  -\sin{\theta} & t_{x}  \\ \sin{\theta} & \cos{\theta} & t_{y} \\ 0 & 0 & 1\end{bmatrix}\bar{x}
$$


두 변환이 같이 일어나는 변환에서 자유도 3

- $t_{x}, t_{y}$, $\theta$



<br><br>

## Similarity Transformation

**Scaling + Rotation + Translation**

유클리디안 변환에 크기 (scale) 변화까지 허용된 변환

선들 사이의 각도가 유지됨



$$
\bar{x}^{'}=\begin{bmatrix} sR &  t\\ 0^{T} &1 \end{bmatrix}\bar{x}=\begin{bmatrix} s\cos{\theta} &  -s\sin{\theta} & t_{x}  \\ s\sin{\theta} & s\cos{\theta} & t_{y} \\ 0 & 0 & 1\end{bmatrix}\bar{x}
$$



자유도는 4

- $t_{x}, t_{y}$, $\theta$, $s$

<br><br>

## Affine Transformation

**Scaling + Rotation + Translation + Shearing + Reflection**

평행한 선들은 변환 후에도 계속 평행을 유지



$$
\bar{x}^{'}=\begin{bmatrix} A &  t\\ 0^{T} &1 \end{bmatrix}\bar{x}=\begin{bmatrix} a_{11} &  a_{12} & t_{x}  \\ a_{21} & a_{22} & t_{y} \\ 0 & 0 & 1\end{bmatrix}\bar{x}
$$



자유도는 6

- $t_{x}, t_{y}$, $a_{11}$, $a_{12}$, $a_{21}$, $a_{22}$ 

<br><br>

## Projective Transformation

같은 말로 perspective transformation, homograhpy

3차원 공간의 평면과 투영 평면과의 관계를 모델링 할 때 사용되는 변환

3차원에서의 평면과 투영 평면에 대한 변환 모델이므로 입체 모델은 나타낼수 없음  

직선은 변환 후에도 계속 직선이 됨


$$
\bar{x}^{'}=H\bar{x}=\begin{bmatrix} h_{11} &  h_{12} & h_{13}  \\ h_{21} & h_{22} & h_{23} \\ h_{31} & h_{32} & h_{33}\end{bmatrix}\bar{x}
$$


자유도는 8

<br>


$$
\begin{bmatrix} h_{11} &  h_{12} & h_{13}  \\ h_{21} & h_{22} & h_{23} \\ h_{31} & h_{32} & h_{33}\end{bmatrix} \propto \begin{bmatrix} a_{11} &  a_{12} & t_{x}  \\ a_{21} & a_{22} & t_{y} \\ v_{1} & v_{2} & 1 \end{bmatrix}
$$



이 때, $$v = [v_{1}, v_{2}]^{T}\neq0$$ 조건을 만족해야 projective transformation 

scaling은 이미지의 위치에 따라 달라질 수 있음



$$
\begin{bmatrix} a_{11} &  a_{12} & t_{x}  \\ a_{21} & a_{22} & t_{y} \\ v_{1} & v_{2} & 1 \end{bmatrix}\begin{bmatrix} x \\ y \\ 1 \end{bmatrix} = \begin{bmatrix} a_{11}x + a_{12}y +t_{x} \\ a_{21}x+ a_{22}y + t_{y} \\ v_{1}x + v_{2}y+1\end{bmatrix}
$$




- scale이 좌표값에 의존하여 변하게 됨



