---
layout: single
title:  "Decorator" # 글 제목
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

## Decorator (데코레이터) 

함수를 꾸미는 함수

즉, 기본적인 함수가 있으면 그 함수를 변형하지 않고 다른 추가적인 기능을 할 수 있게 해주는 보조적인 역할을 하는 함수

closure 인데 그 중에서도 함수를 인자로 받는 closure

**@** 이용하여 사용

Python에서 코드를 깔끔하고 간결하게 만들 수 있게 만들어줌



반복되는 작업을 여러 함수에 적용하거나 기존 함수를 수정하지 않고 추가적인 기능을 구현하고 싶을 때 주로 사용

<br>

### Python 예제

아래와 같이 두 가지 연산의 수행 시간을 측정하는 함수 구현

```python
def func1(n1, n2):
    start = time.time()
    
    val = n1 + n2
    
    end = time.time()
    
    print(f"수행 시간 : {end-start}")
    
    return val

def func2(n1, n2):
    start = time.time()
    
    val = n1 * n2
    
    end = time.time()
    
    print(f"수행 시간 : {end-start}")
    
    return val

```

위와 같이 구현하면 시간을 측정하는 코드가 중복됨

<br>

중복되는 코드를 막기 위해 아래와 같이 decorator를 사용하여 구현

```python
def func1(n1, n2):
    return n1 + n2

def func2(n1, n2):
    return n1 * n2

```

메인 연산 기능을 함수로 구현하고 시간을 측정하는 부분은 아래와 같이 decorator 함수로 작성

```python
'''수행 시간 측정을 위한 데코레이터 '''
def elapsed_time(func): # 함수를 인자로 받음
    
    def wrapper(n1, n2):
        start = time.time()
        result = func(n1, n2)
        end = time.time()

        print(f"수행 시간 : {end-start}")
        
        return result
    
    return wrapper
```

아래와 같이 실행 가능

```python
deco1 = elapsed_time(func1)
result = deco1(1, 2)
print(result)
```

```
수행 시간 : 1.1920928955078125e-06
3
```

```python
deco2 = elapsed_time(func2)
result = deco1(1, 2)
print(result)
```

```
수행 시간 : 1.6689300537109375e-06
2
```

<br>

하지만 보통은 decorator를 정의하는 코드 없이 **@**를 사용하여 바로 적용

```python
@elapsed_time
def func1(n1, n2):
    return n1 + n2

@elapsed_time
def func2(n1, n2):
    return n1 * n2
```

위의 함수를 실행하면 연산을 수행하면서 수행 시간을 측정하는 것을  확인

```python
result1 = func1(1, 2)
print(result1)
result2 = func2(1, 2)
print(result2)
```

```
수행 시간 : 7.152557373046875e-07
3
수행 시간 : 7.152557373046875e-07
2
```

<br>

