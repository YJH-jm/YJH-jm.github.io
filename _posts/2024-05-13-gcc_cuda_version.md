---

layout: single
title:  "[error] UserWarning: There are no g++ version bounds defined for CUDA version 12.1" # 글 제목
categories: 
  - None # category
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

1. [https://github.com/facebookresearch/pytorch3d/issues/1518](https://github.com/facebookresearch/pytorch3d/issues/1518)
1. [https://stackoverflow.com/questions/6622454/cuda-incompatible-with-my-gcc-version/46380601#46380601](https://stackoverflow.com/questions/6622454/cuda-incompatible-with-my-gcc-version/46380601#46380601)

<br><br>

## 문제

`Cuda`  version : 12.1

`torch` version : 2.1.0+cu121 

에서 `torch_scatter` 를 설치하려는데 

<br>



GCC 버전 불일치로 인한 컴파일 오류가 발생하며 아래와 같은 메세지 나옴

```
UserWarning: There are no g++ version bounds defined for CUDA version 12.1
```



Cuda에서 지원하는 최대 [GCC 버전 확인](https://stackoverflow.com/questions/6622454/cuda-incompatible-with-my-gcc-version/46380601#46380601) 을 하고 해당하는 버전으로 gcc version downgrade

Cuda 12.1의 **max supported GCC version**은 12.1인데 현재 설치되어있는 버전은 12.4 였음을 확인

<br>

이를 위해 gcc 11 version 로 downgrade 진행



<br>



### 해결 방법



1. 원하는 버전의 gcc 설치

   ```
   sudo apt-get update 
   sudo apt-get install gcc-11 g++-11
   ```

   

2. gcc default 설정

   ```
   sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 110 
   sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-11 110
   ```

   

3. default 설정 확인 및 변경

   ```
   sudo update-alternatives --config gcc 
   sudo update-alternatives --config g++
   ```



<br>



gcc version을 맞추고 `torch_scatter` 설치하니 아무 문제없이 설치가 되는 것을 확인



<br>



