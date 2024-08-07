---
layout: single
title:  "Type Annotation" # 글 제목
categories: Python # category
tags: [] # tag
toc: true # 오른쪽의 table of contents
toc_sticky: true # 스크롤 내려도 toc 고정
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true # 글 검색 안됨
# use_math: false
typore-root-url: ../
---

****







# Type Annotation

변수 혹은 상수를 선언할 때, 타입을 명시적으로 선언하여서 어떤 타입의 값이 저장될 것인지 표현하는 방법

타입이 잘못되어서 에러 혹은 버그가 발생할 가능성을 줄이고, 코드의 가독성과 협업 효율 증가

필수적이지는 않지만 사용하면 좋음 



Python 3.5 버전 이상부터 지원이 됨



Python은 **동적 프로그래밍 언어** 이고, 프로그램 실행 도중 변수 타입을 동적으로 바꿀 수 있음

이와 반대로 C, C++은 컴파일 할 때, 변수 타입이 결정되어 바꿀 수가 없음



동적 프로그래밍 언어의 장점은 타입에 자유롭기 때문에 유연한 코딩이 가능하며, 쉽고 빠르게 프로그램을 만들 수 있다는 점

하지만 복잡한 코드일 경우, 타입이 맞지 않아 생기는 버그가 발생할 확률이 높고, 원인을 찾기 어려움

특히 C, C++ 과 같이 변수 타입이 결정된 언어와 협업을 하게 되는 경우 이런 문제가 심화됨



<br><br>



아래와 같이 `float` 타입으로 선언한 변수 `a` 에 `str` 타입의 데이터를 넣어도 에러가 발생하지 않음

```python
a = float(30.0)
print(type(a))

a = str("happy")
print(type(a))
```

<br>

```
<class 'float'>
<class 'str'>
```

<br><br>

변수의 type annotation을 명시하는 방법은 아래와 같이 `:` 을 사용

```python
a: float = 30.3 
```

`a` 라는 변수의 타입이 `float` 타입이라는 것을 명시적으로 나타냄



<br>

함수를 만드는 경우에도 아래와 같은 방법으로 가능

```python
def calc(n1: float, n2: float) -> float: 
    return n1 + n2
```

매개 변수로 사용하는 `n1` 과 `n2`가 `float` 타입임을 명시적으로 나타냄

함수에서 return 하는 값의 타입도 `->` 을 사용하여  명시적으로 나타낼 수 있음

<br>

명시적으로 타입을 선언하더라도 함수의 매개변수가 type annotation 으로 나타낸 타입과 다르더라도 타입으로 인한 에러가 발생하지 않음

```python
calc("13", "33")
```

위의 예시의 결과로, 타입으로 인한 에러가 발생하지 않고  `str`의 합으로 결과가 나옴

```
1333
```

<br>

이 때, 함수에 명시적으로 표현한 type annotation을 확인하고 싶으면 아래와 같이 확인 가능 

```python
calc.__annotations__
```

