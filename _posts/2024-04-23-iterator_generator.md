---
layout: single
title:  "Iterator, Generator" # 글 제목
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
use_math: False
typore-root-url: ../
---

**참고**

1. [https://wikidocs.net/16069](https://wikidocs.net/16069)

## Iterable

iterable 객체는 내부 요소를 하나씩 리턴 할 수 있음

즉, for 문을 통해 순회 할 수 있는 객체를 iterable!

```python
  for item in iterable:
      pass
```

<br>

대표적으로 list, tuple, set, dictionary, strting, bytes, range 등이 존재

객체가 iterable한지 알아보고 싶으면 `collections.Iterable`에 속한 instance인지 확인 

```python
import collections

lst = [1, 2, 3, 4]
isinstance(lst, collections.Iterable)
```



<br><br>



## Iterator

![iterable-vs-iterator](/images/2024-04-23-iterator_generator/iterable-vs-iterator.png){:.img-display}

<center style="font-size:12px;">
    <a href="https://nvie.com/posts/iterators-vs-generators/"> 출처 </a>
</center>







Iterator는 복수의 데이터가 들어있는 컨테이터 (list, tuple, set, dictionary, strting) 에서 값을 차례대로 꺼낼 수 있는 객체 (Object)

데이터가 너무 많은 경우 미리 만들어두는 것보다 필요할 때마다 값을 뽑아 사용하는 경우가 대부분의 경우에서 효율적이기 때문에 사용 

하지만 반복이 가능한, 즉 iterable한 객체에서만 사용이 가능

`iter()`로 iterable한 객체로 변환 할 수 있고 변환된 객체는 `next()`로 다음 값 뽑아 사용 가능

한 번 반복을 다 하면, 즉 다음에 뽑을 것이 없으면 다시 사용할 수 없다는 특징이 있음



<br>

### Python 예제

```python
lst = [1, 2, 3, 4]
print(lst.__iter__)
print(lst.__next__)
```

```python
<method-wrapper '__iter__' of list object at 0x766700a98180>
...
AttributeError: 'list' object has no attribute '__next__'
```

이와 같이 list는 iterable한 객체 이지만 iteration한 객체는 아님을 확인



<br>

```python
iterator_lst = iter(lst)

print(type(iterator_lst))
print(iterator_lst.__iter__)
print(iterator_lst.__next__)
```

```python
<class 'list_iterator'>
<method-wrapper '__iter__' of list_iterator object at 0x766701a3f4c0>
<method-wrapper '__next__' of list_iterator object at 0x766701a3f4c0>
```

`iter()` 내장 함수를 통해 iteration 객체로 변환시키면 type이 바뀌는 것을 확인 할 수 있고`__next__ `  매직 매소드를 가지게 된 것을 확인

<br>

```python
print(next(iterator_lst))
print(next(iterator_lst))
print(next(iterator_lst))
print(next(iterator_lst))
print(next(iterator_lst))
```

```python
1
2
3
4
....
StopIteration: 
```

`next()`를 통해 값을 확인해보면 순차적으로 값이 나오다가 나올 값이 없으면 에러 발생

<br><br>

## Generator

Generator는 iterator를 생성해주는 함수

즉, generator는 특이한 iterator라고 볼 수 있음 (반대는 성립하지 않음)

보통 다음 값을 필요에 따라 계산이 되고 함수 내부 로컬 변수를 통해 내부 상태가 유지됨 

함수가 하나의 고정된 값을 반환하는 것이 아니라 순차적으로 다른 값을 반환해야할 때 사용하거나 상황마다 다른 값을 return 해야 할 때, return 하는 시점을 다르게 하고 싶을 때 사용 

Generator는 `yield()` 을 이용하여 생성할 수 있으며 흔하게 알고 있는 list comprehension 등의 표현식도 generator 

`yield()`가 호출이되면 암시적으로 return이 호출되고, 한 번 더 실행되면 `yield()` 다음 코드 실행

<br>

### Python 예제

```python
def generator():
    yield 'a'
    yield 'b'
    yield 'c'
    
g = generator()
type(g)
```

```
generator
```

`yield`를 사용하면 generator type이 되는 것을 확인

```python
print(next(g))
print(next(g))
print(next(g))
print(next(g))
```

```python
a
b
c
...
StopIteration: 
```

generator 역시 순차적으로 값이 나오다가 나올 값이 없으면 에러가 발생함

<br>

가게에서 웨이팅 하는 손님에게 몇 번째 웨이팅 손님 입장하라고 알려주는 코드를 작성할 때도 쓸 수 있음

```python
def waiting_count(max_count):
    cnt = 1
    
    for num in range(max_count):
        print(f'{cnt} 번 손님 입장 가능')
        cnt += 1
        yield
        
generator = waiting_count(10)

next(generator)
next(generator)
```

```
1 번 손님 입장 가능
2 번 손님 입장 가능
```

Generator를 이용하여 코드를 작성하면 시점에 맞게 실행할 수 있음

즉, 입장이 가능해져 이 message를 전송하려고 할 때 보낼 수 있음

<br><br>
