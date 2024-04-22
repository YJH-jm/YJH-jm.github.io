---
layout: single
title:  "Encoding, Decoding" # 글 제목
categories: 
  - Python # category
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

## Encoding 과 Decoding

![encoding_decoding](/images/2024-04-21-encoding_decoding/encoding_decoding.png){:.img-display}

<center style="font-size:12px;">
    <a href="https://sourcedexter.com/data-encodingdecoding-in-python/"> 출처 </a>
</center>

<br>

### Encoding (인코딩)

사람이 인지할 수 있는 형태의 데이터 (문자열, 동영상 등)를 컴퓨터가 사용하는 0과 1로 변환하는 과정

변환할 때는 약속된 규칙에 따름

ASCII, URL 인코딩, HTML 인코딩, Base64 인코딩, 유니코드 인코딩 등 존재

인코딩은 다른 말로 코드화, 암호화, 부호화 라고도 함

<br>

### Decoding (디코딩)

인코딩한 형태의 데이터를 다시 되돌라는 과정

즉, 사람이 이해할 수 있는 형태의 데이터로 되돌리는 과정

디코딩은 다른 말로 역코드화 복호화 라고 함

<br>

### Python 예제

```python
a = "Python! _ "
print(type(a))


b = a.encode('utf-8') # 유니코드 인코딩
print(b)
print(type(b))
```

```
<class 'str'>
b'Python! _ '
<class 'bytes'>
```



<br>

```python
a = "파이썬! _"
b = a.encode('utf-8')
print(b)
```

```
b'\xed\x8c\x8c\xec\x9d\xb4\xec\x8d\xac! _'
```

<br>

인코딩한 방식과 다른 방식으로 디코딩하면 에러가 발생

```python
b.decode('ascii')
```

```python
UnicodeDecodeError: 'ascii' codec can't decode byte 0xed in position 0: ordinal not in range(128)
```

<br>

Python 2.X 버전은 ascii 가 기본 인코딩 방식이지만

Python 3.X 버전은 utf-8 가 기본 인코딩 방식 

따라서 소스 코드 파일이 현재 어떤 방식으로 인코딩 되어있는지 명시해주는 것이 좋음

특히나 편집기로 코딩할 때는 소스 코드의 인코딩이 매우 중요함

<br>

Python 2.X 버전에서 utf-8 로 인코딩 된 문장 실행할 때 아래와 같이 utf-8로 인코딩했다는 것을 명시해주면 프로그램이 알아서 인코딩 방식 바꿔 실행

소스 코드 맨 위에 적어야 함

```python
# -*- coding: utf-8 -*-
a = '한글이 보입니다!'
```



