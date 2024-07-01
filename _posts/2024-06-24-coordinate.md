---

layout: single
title:  "좌표계" # 글 제목
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

1. [좌표계 관련 ](https://darkpgmr.tistory.com/77?category=460965)

<br><br>



# Coordinate System

좌표계에는 왼손 좌표계와 오른손 좌표계가 존재

<br>

![3](/images/2024-06-24-coordinate/3.png){:.img-display}

<center style="font-size:12px;">
    <a href="https://sonagi87174.tistory.com/28"> 출처 </a>
</center>

<br>

참고로 pytorch3D, OpenCV, OpenGL 모두 오른손 좌표계를 사용하나, Unity의 경우 왼손 좌표계를 사용

<br>

전체적인 좌표계의 관계는 다음과 같음

<br>

![coord](/images/2024-06-24-coordinate/coord.png)

<center style="font-size:12px;">
    <a href="https://darkpgmr.tistory.com/77?category=460965"> 출처 </a>
</center>



<br><br>

## 월드 좌표계 (World Coordinate System)

우리가 살고 있는 공간의 한 지점을 기준으로 한 좌표계 

즉, 물체의 위치를 표현하기 위해 임의로 기준을 잡은 좌표계

방의 모서리 위치,  물체의 현재 위치 등 임의로 기준을 잡아서 사용

단위는 상관이 없으나, 월드 좌표계에서는 한 위치에 대한 표현이 하나로 정해져야 함

<br>


$$
P=(X,Y,Z)
$$
<br><br>

## 카메라 좌표계 (Camera Coordinate System)

카메라를 기준으로 한 좌표계

카메라의 초점(렌즈의 중심)을 원점, 광학축 방향을 Z축, 카메라의 아래쪽 방향을 Y, 오른쪽방향을 X축으로 잡음

월드 좌표계의 단위와 카메라 좌표계의 단위는 동일해야 함

<br>


$$
P_{C}=(X_{C}, Y_{C}, Z_{C})
$$

<br><br>

## 이미지 좌표계 (Image Coordinate System)

픽셀 좌표계 (Pixel Image Coordinate System) 라고도 함

<br>



$$
p=(x,y)
$$



<br>

3D 공간상의 한 점 $P$는 카메라의 초점 (또는 렌즈의 초점)을 지나서 이미지 평면의 한 점 [![p_{img}=(x,y)](https://camo.githubusercontent.com/cc6edbbdc29460eeb13e3e9038bef2e5b619d0a5d27b2042279f43bb8f38aadf/68747470733a2f2f6c617465782e636f6465636f67732e636f6d2f7376672e696d6167653f705f7b696d677d3d28782c7929)](https://camo.githubusercontent.com/cc6edbbdc29460eeb13e3e9038bef2e5b619d0a5d27b2042279f43bb8f38aadf/68747470733a2f2f6c617465782e636f6465636f67732e636f6d2f7376672e696d6167653f705f7b696d677d3d28782c7929)에 투영(projection)

원점은 이미지의 좌상단의 점이고 이미지의 해상도의 크기를 가짐

- 해상도는 카메라에  따라 다름

<br><br>

## 정규 이미지 좌표계 (Normalized Image Coordinate System)

카메라 내부 파라미터에 대한 영향을 제거한 이미지 좌표계

좌표계의 단위가 없는 정규화 된 좌표계이며 **카메라 초점과의 거리가 1**인 가상의 이미지 평면을 정의하는 좌표계

<br>

$$
x=(u,v)
$$




- 원점은 정규 이미지 평면의 중점(평면과  [![Z_{C}](https://camo.githubusercontent.com/6fb0931690f73b6a2cee9370c890935b0e9ef42f5e7a96008d31093673a2acff/68747470733a2f2f6c617465782e636f6465636f67732e636f6d2f7376672e696d6167653f5a5f7b437d)](https://camo.githubusercontent.com/6fb0931690f73b6a2cee9370c890935b0e9ef42f5e7a96008d31093673a2acff/68747470733a2f2f6c617465782e636f6465636f67732e636f6d2f7376672e696d6167653f5a5f7b437d)의 교점)

<br><br>
