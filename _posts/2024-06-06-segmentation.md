---

layout: single
title:  "Segmentation 개요" # 글 제목
categories: 
  - Object Tracking # category
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

1. [An overview of semantic image segmentation](https://www.jeremyjordan.me/semantic-segmentation/)
1. [Semantic Segmentation vs. Instance Segmentation: Explained](https://blog.roboflow.com/difference-semantic-segmentation-instance-segmentation/)

<br><br>

# Segmentation

이미지의  픽셀마다 각 영역이 어떤 class 인지 구별하는 기술

모든 pixel에 대해서 예측을 하기 때문에 dense prediction 이라고 하기도 함



![Screen-Shot-2018-05-17-at-9.02.15-PM](/images/2024-06-06-Segmentation/Screen-Shot-2018-05-17-at-9.02.15-PM.png){:.img-display-60}

<center style="font-size:12px;">
    <a href="https://www.jeremyjordan.me/semantic-segmentation/"> 출처 </a>
</center>

<br>



## Segmantiotn 종류

### Semantic Segmentation

픽셀들이 각 class에 대해 포함이 되는지 아닌지 여부만 따짐

즉, 픽셀 레벨에서 image classification task를 수행한다고 생각

같은 class의 object는 구분되지 않음

<br>

![Screenshot from 2024-06-07 01-16-17](/images/2024-06-06-Segmentation/Screenshot from 2024-06-07 01-16-17.png){:.img-disply-60}

<center style="font-size:12px;">
    <a href="https://wiki.math.uwaterloo.ca/statwiki/index.php?title=User%3AX93ma&ref=blog.roboflow.com"> 출처 </a>
</center>

<br>

### Instance Segmentation

같은 class여도 각 object로 구분이 가능한 Segmentation 방법

보통 localization을 수행한 후, 그 box의 object들에 대하여 segmentation을 진행

<br>



![Screenshot from 2024-06-07 01-16-49](/images/2024-06-06-Segmentation/Screenshot from 2024-06-07 01-16-49.png){:.img-disply-60}

<center style="font-size:12px;">
    <a href="https://wiki.math.uwaterloo.ca/statwiki/index.php?title=User%3AX93ma&ref=blog.roboflow.com"> 출처 </a>
</center>

<br>

## Segmentation 평가 지표

### Pixel Accuracy (PA)

올바르게  class를 예측한 픽셀 수의 비율


$$
\text{PA}=\frac{\sum_{i=0}^{K}p_{ii}}{\sum_{i=0}^{K}\sum_{j=0}^{K}p_{ij}}=\frac{TP+TN}{TP+FN+TN+FP}
$$


- $p_{ij}$ : $i$ class (GT)를 $j$ class로 예측한 픽셀의 수

<br>

### Mean Pixel Accuracy (MPA)

PA를 class 별로 구하여 평균한 값


$$
\text{MPA}=\frac{1}{K+1}\sum_{i=0}^{K}\frac{p_{ii}}{\sum_{j=0}^{K}p_{ij}}
$$


- $\sum_{j=0}^{K}p_{ij}$ : 각 class의 pixel 수

<br>

### Intersection over Union (IoU)

Segmentation에서는 Jaccard Coefficient 와 같은 의미


$$
\text{IoU}=\frac{|A\cap B|}{|A\cup B|}=\frac{TP}{TP+FP+FN}
$$


<br>

#### mIoU 

class 별 IoU 값의 평균


$$
\text{mIoU}=\frac{1}{K+1}\sum_{i=0}^{K}\frac{TP_{i}}{TP_{i}+FP_{i}+FN_{i}}
$$


<br>

#### Frequency Weighted IoU (FWIoU)

Class 별로 전체 픽셀에서 해당 class의 픽셀 비율을 가중치로 적용한 값 

자주 나타내는 class에 가중을 둠


$$
\text{FWIoU}=\sum_{i=0}^{K}\frac{P_{i}}{P_{total}}\ \frac{TP_{i}}{TP_{i}+FP_{i}+FN_{i}}
$$


- $P_{total}$, $P_{i}$ : 각각 전체 pixel 수, $i$ class의 pixel 수

<br>

### Dice Coefficient

F1 score와 동일한 의미로 사용

- Medical Image에서 많이 사용


$$
\text{Dice}=\frac{2|A\cap B|}{|A|+|B|}=\frac{2TP}{(TP+FP)+(TP+FN)}=\frac{2TP}{2TP+FP+FN}
$$


<br>

### Hausdorff distance (HD)

두 집합이 존재할 때, 한 집합의 점을 기준으로 다른 쪽 집합까지의 가장 먼 거리를 구하고 그 반대도 실행한 후 그 중 더 큰 값을 구함

즉, 한 집합이 어떤 점에서 다른 집합까지 도달하기 위한 거리의 최댓값 

<br>

![Hausdorff_distance_sample.svg](/images/2024-06-06-Segmentation/Hausdorff_distance_sample.svg.png){:.img-display}

<center style="font-size:12px;">
    <a href="https://en.wikipedia.org/wiki/Hausdorff_distance"> 출처 </a>
</center>

<br>




$$
\begin{align*}
d_{h}(X,Y)&=\text{max}\{d_{XY},d_{YX}\} \\
&=\text{max}\{\text{max}_{x\in X}\text{min}_{y\in Y} \ d(x,y) , \text{max}_{y\in Y}\text{min}_{x\in X} \ d(x,y) \}\\
&=\text{max}\{\text{sup}_{x\in X}\text{inf}_{y\in Y} \ d(x,y) , \text{sup}_{y\in Y}\text{inf}_{x\in X} \ d(x,y)\}
\end{align*}
$$


- $X$ : Ground truth
- $Y$ : prediction
- $d(\cdot )$ :  distance

<br>

식을 살펴보면


$$
d_{XY}=\text{max}_{x\in X}\text{min}_{y\in Y} \ d(x,y)
$$


$x$의 임의의 한 점과 모든 $y$와의 거리를 구하여 가장 짧은 거리를 구함, 이 과정을 모든 $x$ 값에 대하여 반복했을 때, 찾은 최소의 거리 중 가장 큰 값

 

<br>
$$
d_{YX}=\text{max}_{y\in Y}\text{min}_{x\in X} \ d(x,y)
$$


$y$의 임의의 한 점과 모든 $x$와의 거리를 구하여 가장 짧은 거리를 구함, 이 과정을 모든 $y$ 값에 대하여 반복했을 때, 찾은 최소의 거리 중 가장 큰 값

<br>



위의 두 값 중 더 큰 값을 선택

<br>

### Average Hausdorff Distance (AVD)

HD는 outlier에 매우 취약한데, 이를 보완하기 위한 방법으로 선택

최소의 거리 중 가장 큰 거리 값을 찾는 과정에서 outlier에 취약해지기 때문에 이를 보완하기 위해서 max가 아닌 평균값을 사용


$$
d_{\text{AH}}(X,Y)=\frac{1}{|X|}\sum_{x\in X}\text{min}_{y\in Y} \ d(x,y)+ \frac{1}{|X|}\sum_{y\in Y}\text{min}_{x\in X} \ d(x,y) \}\\
$$
