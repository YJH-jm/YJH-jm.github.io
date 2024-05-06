---

layout: single
title:  "3D object detection 개요" # 글 제목
categories: 
  - 3D Object Detection # category
tags: 
  - [point cloud] # tag
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

## 3D Oject Detection

### LiDAR-based 3D Object Detection

LiDAR 데이터를 이용하여 3차원 object detection 가능

2D object detection 대비 많은 메모리와 컴퓨팅 성능이 요구됨

출력 결과는 **bounding cube** 형태로 구성됨

- $x$, $y$, $z$, $w$, $h$, $l$

Bouding cude의 좌표값 외에 방향 정보를 얻도록 학습할 수 있음

-  자율 주행의 경우 자동차가 어떤 방향으로 향하고 있는지 알아야 하는 경우

<br>

#### 입력 데이터에 대한 방법론

##### Voxel 기반

보편적인 방법

voxel grouping을 통해 데이터를 묶어 네트워크 학습을 진행

대표적으로 VoxelNet, SECOND, PointPillars, PV-RCNN 등이 존재

<br>

##### Point 기반

입력 point cloud를 직접 사용하는 방법

대표적으로는 IPOD, PointRCNN, STD, LasetNet 등이 존재

<br>

##### Projection 기반

2차원 projection을 통해 2차원 이미지와 같은 데이터 형태로 만드는 것 가능

원하는 view를 선택하여 projection 가능

자율 주행과 같은 경우는 bird eye view projection 선호

대표적으로는 HDNet, BirdNet, RT3D, Pixor 등 존재

<br>

##### Frustum 기반

공간을 절단하여 평행한 평면을 얻어서 객체 검출을 수행하는 방법

대표적으로는 Frustum-PointNet, Frustum-ConvNet, SIFRNet 등 존재

<br>



