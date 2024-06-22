---

layout: single
title:  "3D segmentation 개요" # 글 제목
categories: 
  - 3D Vision # category
tags: 
  - [] # tag
toc: true 
toc_sticky: true
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true 
use_math: false
typore-root-url: ../
---

**참고**

1. [논문](https://arxiv.org/pdf/1912.13192)

<br><br>

## 3D segmentation 방법론

3D segmentation 방법은 크게 다섯 가지 존재

<br>

### Edge-based segmentation

3차원 point cloud 내에서 불연속적인 edge 영역을 찾는 것에 중점을 둔 방법

Edge는 주로 물체의 경계나 표면의 큰 변화가 발생하는 영역에서 발생

인접한 point rksdml normal vector 차이와 gradient를 계산하여, 그 차이가 크면 edge라고 간주할 수  있음

Object의 경게를 직접 targeting하여 계산적으로 효율적

하지만 Edge에 존재하는 noise에 민감하고, 이로 인해 정제를 위한 후처리를 다른 방식들에 비해 많이 요구

<br>

### Region-based segmentation

유사한 특징을 가지는 point들끼리 그룹으로 묶어 일관된 지역을 형성하는데 중점을 둔 방법

유사한 특징으로 묶기 위해 clustering 알고리즘 사용

- K-means, DBSCAN 등

Region growing을 사용하여 명확하게 객체의 영역 묶음

- Region growing은 유사한 속성을 공유하는 인접 point를 추가하여 시드를 확장하는 방법

보통 noise에 강인하고, 부드럽고 일관된 segmentation 결과 획득 가능

하지만 유사한 속성을 가지는 개별 객체를 잘못 병합할 수 있음

- 고양이 옆에 있는 호랑이를 고양이로 잘못 병합

<br>

### Model-based segmentation

사전 정의된 모델 혹은  템플릿을 사용하는 방법

- 3D CAD 모델을 사전 정보로 사용하는 등의 방법

미리 정의된 모델을 RANSAC과 같은 방법으로 target point cloud 데이터에 맞추면서 진행

알려진 형상에 대해서는 높은 정확도로 segmentation 가능하고 사전 정보를 바탕으로 해당 객체의 매개 변수를 직접 제공 가능

하지만 알려지지 않은 모델에 대해서는 정확도가 떨어짐

<br>

### Attributes-based segmentation

Point들의 다양한 속성을 기반으로 분할하는 방법

- 색상, 밀도, 곡률 등

Deep learning 기반으로 point cloud feature를 추출하여 segmentation 가능

미묘한 차이를 포착하는 것이 가능

다양한 속성을 기반으로 다양한 작업에 적용이 가능하지만 feature 추출을 해야 하기 때문에 계산 비용이 많이 발생

<br>

### Graph-based segmentation

Point cloud를 그래프로 표현하여 접근하는 방법

고전적인 알고리즘으로는 그래프-컷 알고리즘과 같은 방법으로 segmentation

Segmentation 내 차이는 최소화하고, Segmentation 간 차이는 최대화 하는 방법

Class가 다양한 대규모 데이터 셋에 활용하기 좋은 방법이라고 주장

그래프 구조, edge의 가중치 등을 어떻게 정의해야 하는지에 대한 제약 존재

