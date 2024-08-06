---
layout: single
title:  "Docker 딥러닝 실행 " # 글 제목
categories: Docker # category
tags: [docker] # tag
toc: true # 오른쪽의 table of contents
toc_sticky: true # 스크롤 내려도 toc 고정
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true # 글 검색 안됨
use_math: ture
typore-root-url: ../
---

****

Dockerdhk Nvidia Driver가 설치된 후 진행

docker run -d -p 8787:8787 --gpus all -v "< local path >:< container path >" --name < container name > < image name > /bin/bash -c "rstudio-server start && tail -f /dev/null"  

```
docker run -d -p 8787:8787 --gpus all -v "< local path >:< container path >" --name < container name > < image name > /bin/bash -c "rstudio-server start && tail -f /dev/null"  

docker run -d -p 8787:8787 --gpus all -v /home/azureuser_jh/rproject/:/home/rstudio/rproject --name
```

- `-d` , `--detach`
  - Container가 background에서 동작
  - Container에 계속 접속되는 것이 아닌, 내 터미널로 반환
- `-v`
  - 로컬 디렉토리와 컨테이너의 디렉토리 연결하는 내용
- `-p`
  - 
  - 포트를 연결해주어야하는데 가상머신에서 8787 포트 사용하는데
  - 그 포트를 우리의 local 환경에서 사용할 수 있도록 해야함
  - 8787번 

- `--gpus`
  - gpu device 정의

- `--name`
  - Container 이름 부여
