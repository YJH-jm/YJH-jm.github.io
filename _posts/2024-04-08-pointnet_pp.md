---
layout: single
title:  "PointNet++: Deep Hierarchical Feature Learning on Point Sets in a Metric Space" # 글 제목
categories: 
  - 3D Vision # category
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

NIPS 2017

[논문](https://arxiv.org/pdf/1706.02413.pdf)

<br>

## Introduction

 기존의 PointNet은 각 point의 spatial encoding을 학습하고 모든 포인트들의 특징을 합쳐 전역적인 point cloud의 특성을 학습

이러한 특징 때문에 지역적(local)인 특징을 학습하기 어려움

<br>

지역적 특징을 추출하기 위해 CNN과 유사한 개념의 계층형 네트워크 구조인 **PointNet++** 제안

이를 위해 point들을 겹쳐지는 local region으로 나누고 CNN 처럼 적은 neighborhoods로 (근처의 pointcloud) 지역적인 특징을 추출함

그 후 higher level의 특징을 얻기 위해 큰 그룹으로 합치고 이 과정을 모든 point set의 특징을 얻을 때까지 반복

<br>

 이런 네트워크를 사용하기 위한 2가지 해결해야하는 이슈가 존재

1. Local Region
   - CNN과 같이 지역적 특징을 얻기 위해서는 전체 point cloud의 영역을 나누어야 함
   - Neighborhood ball 구조를 이용하여 나눔 
2. Local Feature
   - Weight를 공유하며 계층적으로 지역적인 특징부터 전체적인 semantic 특징까지 학습하는 방법 필요
   - PointNet를 local region에 적용해 계층적으로 점점 고차원 특징을 학습하는 방법 제시



<br><br>

## Method

![Screenshot from 2024-04-08 12-23-59](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 12-23-59.png)

<br>

### Hierarchical Point Set Feature Learning

 계층적인 구조는 많은 *set abstraction* level로 구성

각 level은 Sampleing layer, Grouping layer, PointNet layer로 구성

Sampling layer는 입력 point에서 Local region의 centroid를 정의하는 포인트들을 선택 

Grouping layer는 centroid 주변  포인트들을 기반으로 local region을 형성

PointNet layer는 local region을 인코딩

<br>

#### Sampling layer

입력 point에서 local region을 만들기 위한 point subset(centroid)들을 FPS (Farthest point Sampling)을 통하여 선정



![Screenshot from 2024-04-08 12-49-29](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 12-49-29.png){:.img-display}

<center style="font-size:12px;">
    <a href="https://medium.com/@parkie0517/pointnet-deep-hierarchical-feature-learning-on-point-sets-in-a-metric-space-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-519f6fe2b888"> 출처 </a>
</center>

Random Sampling 대비 loss 수렴에 더 좋은 결과를 얻음

FPS를 사용하면 골고루 point를 선정하게 되며 데이터 의존적인 receptive field를 만드는 효과 존재



<br>

#### Groping Layer

$ N \times (d+C) $의 point 집합과 $ N^{\'} \times d$  중심점 집합 

- $C$ : point 특징의 차원
- $N$, $d$: 전체 point의 수와 각 point의 좌표 차원
- $ N^{\'}$ : centroid 수

$ N^{'} \times K \times (d+C)$ 크기의 point set 그룹을 출력으로 함 

- $K$ : 각 centroid가 가지는 이웃한 point 수, 그룹마다 다를 수 있음 



<br>



CNN은 하나의 픽셀로 부터 특정 Mehattan distance (=kernel size) 만큼의 위치한 픽셀들까지 포함하여 local region을 정의

PointNet++에서는 이웃한 metric 거리에 있는 metric space로 정의

<br>

Sampling 과정에서 얻은 centroid를 중심으로 하고 radius 안에 들어오는 ball query 형성하고 구의 반경 내에 존재하는 point들을 하나의 그룹으로 형성

- 구현시 $K$의 상한값 설정 

<br>

$K$ 값이 고정된 KNN 알고리즘을 사용하지 않고 Ball query 사용하는 이유는 고정된 region scale을 보장하여 local region feature를 공간상에서 더 일반화 할수 있음 

또한 PointNet을 사용하기 때문에 ball query 내 point의 수가 달라도 활용이 가능

<br>

#### PointNet layer

$N^{\'}$개의 local region들의 point들이 입력이며 그 데이터의 크기는 $ N^{\'} \times K \times (d+C)$ 

출력은 각 local region의 centroid의 이웃 point들을 인코딩한 local feature와 centroid 점에 의해 요약되고 크기는 $ N^{\'} \times (d+C^{\'})$ 

<br>

Local region의 point들은 중심 좌표에 대한 상대적인 좌표로 변환 한 후

각 Local region에 대하여 PointNet 적용

상대 좌표를 point 특징과 함께 사용하면 local region에서 point-to-point 간의 관계를 알 수 있음 

<br>

###  Robust Feature Learning under Non-Uniform Sampling Density

Point cloud의 데이터 밀도, 분포는 일정하지 않음

Dense한 영역 위주로 학습을 하면 Sparse한 data에 대한 학습 일반화가 매우 어렵고 Sparse한 영역 위주로 학습하면 Fine-grained한 지역적 구조 인식이 어려움

이 문제를 해결하기 위해  **Density adaptive PointNet Layer** 적용함

 Density adaptive PointNet Layer를 적용한 계층적인 네트워크를 **PointNet++**라 부름



![Screenshot from 2024-04-08 14-19-00](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 14-19-00.png){:.img-display-60}

<br>

#### Multi-scale grouping (MSG)

다양한 크기로 grouping하는 layer를 거친 후  각 scale의 feature를 뽑기 위해 PointNet을 적용하고 그 결과를 concat 

Multi-scale 특징을  잘 학습하기 위해  dropout을 적용하여 다양한 sparsity를 가짐



<br>



#### Multi-resolution grouping (MRG)

MSG 방식은 모든 중심점에서 많은 수의 주변 point들에 대하여 local PointNet을 적용해야 하기 때문에 계산의 효율이 좋지 않음

특히 앞 단계일수록 중심점의 수가 많기 때문에 계산의 효율이 더 좋지 않음 

<br>

이 문제를 해결하기 위해 MRG 적용 

Sub-region의 feature vector와 raw point의 feature vector를 각각 뽑아 concat

각 subregion의 특징을 set abstratction을 사용하여 각 local region의 특징을 요약하여 sub-region feature 획득

local region의 모든 raw point들을 하나의 PointNet을 통과시켜  raw point feature 획득

예를 들어 PointNet++에서 의 set abstration을 두 번 진행하면 처음 진행하여 얻은 feature 부터 마지막 진행하여 얻은 feature vector까지 concat하여 여러 level의 정보를 활용 ...? 

Local region의 density가 낮으면  subregion에 대한 feature vector가 raw point에 대한 feature vector 보다 신뢰도가 낮아지기 때문에 raw point에 대한 vector의 weight를 더 부여



### Point Feature Propagation for Set Segmentation

Classification을 위해 점들이 subsample

Segmentation의 경우는 다시 복구하여 원래 점들의 하나하나 label을 파악해야 함

항상 모든 점을 중심점으로 샘플링하는 방법으로 해결 할 수 있으나 이는 계산 비용이 매우 높음

다운 샘플링된 point로 부터 원본 point까지 feature를 전파하는 방법 선택

위의 그림처럼 거리 기반의 interpolation과 level skip link를 통한 계층적 propagation 방법을 사용



<br><br>



## Experiments

### Point Set Classification in Euclidean Metric Space

#### Result 

![Screenshot from 2024-04-08 15-16-08](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-16-08.png){:.img-display}

<br>

![Screenshot from 2024-04-08 15-19-19](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-19-19.png){:.img-display}

- normal vector를 추가하면 분류 정확도가 더 높아짐을 확인

<br>

#### Robustness to Sampling Density Variation

![Screenshot from 2024-04-08 15-24-13](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-24-13.png)

- Density adaptive 전략의 중요성 확인 가능

- Point 수가 줄어들더라도 SSG(Single Scale Grouping) 방식과 다르게 정확도의 감소가 현저하게 덜 함

<br>

### Point Set Segmentation for Semantic Scene Labeling

#### Robustness to Sampling Density Variation 

![image-20240408153024983](/images/2024-04-08-pointnet_pp/image-20240408153024983.png){:.img-display}

<br>

![Screenshot from 2024-04-08 15-42-20](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-42-20.png){:.img-display}

- PointNet 과 비교하여 객체 식별 능력 향상

<br>

### Point Set Classification in Non-Euclidean Metric Space

![Screenshot from 2024-04-08 15-44-54](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-44-54.png){:.img-display}

- Animation 처럼 움직이고 있는, motion에 대한 data도 같이 들어가 있음

![Screenshot from 2024-04-08 15-46-06](/images/2024-04-08-pointnet_pp/Screenshot from 2024-04-08 15-46-06.png){:.img-display-60}

- 단순 XYZ 좌표가 아닌 PointNet++ 내부 feature를 사용한 경우 성능이 더 올라감 

<br><br>

## Conclusion

CNN 특징을 최대한 유사하게 활용할 수 있는 계층적인 구조 제안

Local feature를 얻기 위해 Local region을 나누어 계층적으로 PointNet을 적용

Density가 균일하지 않다는 문제를 해결하기 위해 MSG, MRG를 적용

PointNet과 비교하여 성능 향상

<br><br>

참고

1. [https://jaehoon-daddy.tistory.com/47](https://jaehoon-daddy.tistory.com/47)

   

<br><br>

