---
title: "Numpy"
excerpt: "Numpy에 대하여"
categories: 

- Python
tags: 
- [Python]
- [data]
date: 2022-03-23 12:10:00

---

# Numpy(넘파이)

Numpy는 파이썬의 라이브러리로 벡터, 행렬 등 수치 연산을 수행하는 선행대수 라이브러리 이다.

```python
pip install numpy # 넘파이가 없을 경우 설치
import numpy as np # 추가 방법
```
넘파이 라이브러리가 없을 경우 추가 및 설치 방법

## 배열 만들기

```python
temp = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
arr = np.array(temp)
print(arr)
print(temp)

# [ 1  2  3  4  5  6  7  8  9 10]
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
```python
print(type(temp))
print(type(arr))

# <class 'list'>
# <class 'numpy.ndarray'>
```

### 0~3차원 배열 만들기
* 0차원 배열
```python

temp_arr = np.array(10)
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape)
# 10
# <class 'numpy.ndarray'>
# ()
```
* 1차원 배열
```python
# 1차원 배열
temp_arr = np.array([1, 2, 3])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape)
print(temp_arr.ndim)
# [1 2 3]
# <class 'numpy.ndarray'>
# (3,) # x가 3, y는 없어서 출력되지 않는다. 순서가 다르다.
# 1 # 1차원 이라는 뜻

```
* 2차원 배열
```python
temp_arr = np.array([[1, 2, 3], [4, 5, 6]])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape)
print(temp_arr.ndim)
# [[1 2 3]
# [4 5 6]]
# <class 'numpy.ndarray'>
# (2, 3) # y가 2, x가 3이라는 뜻이다.
# 2  # 2차원이라는 뜻
```
* 3차원 배열
```python
temp_arr = np.array([[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]])
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape)
print(temp_arr.ndim)
[[[1 2 3]
  [4 5 6]]

 [[1 2 3]
  [4 5 6]]]
<class 'numpy.ndarray'>
(2, 2, 3) # z가 2 ,y가 2 , x가 3이라는 뜻이다.
3 # 3차원

```
* 최소 차원 수 지정
```python
temp_arr = np.array([1, 2, 3, 4], ndmin = 2) # ( ndmin = 2) 최소 차원을 2차원으로 설정한다.
print(temp_arr)
print(type(temp_arr))
print(temp_arr.shape)
print(temp_arr.ndim)
[[1 2 3 4]]
<class 'numpy.ndarray'>
(1, 4) # y가 1 x가 4 
2 # 2차원
```
### 배열을 생성하는 다양한 방법
```python
temp_arr = np.arange(5) # 0~4까지
temp_arr
# array([0, 1, 2, 3, 4])
```
```python
temp_arr = np.arange(1, 15, 2) # 1~15까지 2간격으로
temp_arr
# array([ 1,  3,  5,  7,  9, 11, 13])
```
```python
zero_arr = np.zeros((2, 3)) # 0으로 이루어진(zeors) 2 * 3의 배열을 생성한다.
print(zero_arr)       # 배열 출력  
print(type(zero_arr)) # 배열의 타입
print(zero_arr.shape) # 배열의 크기
print(zero_arr.ndim) # 차원
print(zero_arr.dtype) # 배열의 자료형
``` 
```python
temp_arr = np.ones((12, 12), dtype="int32") # 1로 이루어진(ones) 12 * 12의 배열을 생성하고, 자료형을 "int32"로 설정
temp_res_arr = temp_arr.reshape(8, -1)  # reshape는 배열의 차원과 모양을 변경한다. -1을 붙일 경우 알아서 크기를 조정한다.
print(temp_res_arr) # 배열 출력  
print(type(temp_res_arr)) # 배열의 타입
print(temp_res_arr.shape) # 배열의 크기
print(temp_res_arr.ndim) # 차원
print(temp_res_arr.dtype) # 배열의 자료형
```


## 사칙연산과 기초통계
```python
math_scores = [2, 3, 4]
english_scores = [1, 2, 3]

math_arr = np.array(math_scores)
english_arr = np.array(english_scores)
# 사칙연산
print("덧셈:", np.add(math_arr, english_arr))
print("뺄셈:", np.subtract(math_arr, english_arr))
print("곱셈:", np.multiply(math_arr, english_arr))
print("나눗셈:", np.divide(math_arr, english_arr))
print("거듭제곱:", np.power(math_arr, english_arr))
```
넘파이 배열은 +, - , * , / 등 사칙 연산이 가능하다. 

```python
print(np.mean(math_scores))
print(np.median(math_scores))
print(np.std(math_scores))
```

또한 mean,median,std 등 통계 함수가 내장 되어 있다. [자세한 정보는..](https://numpy.org/doc/stable/reference/routines.statistics.html)

## Numpy의 조건식

* np.where
```python
temp_arr = np.arange(5)
temp_arr
# array([0, 1, 2, 3, 4])
```
```python
np.where(temp_arr < 3, temp_arr, temp_arr * 10) # 1개의 조건식
# 3보다 작은 값은 원래 값으로 반환된다.
# 3보다 큰 값은 원래 값 * 10
# array([ 0,  1,  2, 30, 40])
```
* np.select (2개이상)
```python
temp_arr = np.arange(10)
temp_arr
# array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```
```python
condlist = [temp_arr > 6, temp_arr < 3]  # 6보다 큰값 * 4 , 3보다 작은값 + 10
choielist = [temp_arr * 4, temp_arr + 10] 
np.select(condlist, choielist, default = temp_arr)
# array([10, 11, 12,  3,  4,  5,  6, 28, 32, 36])
```

## 소숫점 이쁘게

```python
temp_arr = np.ceil([-1.23456, 1.23456]) # 올림 # 입력값과 같거나 큰 정수중 가까운 값
temp_arr
# array([-1.,  2.])
```
```python
temp_arr = np.floor([-1.23456, 1.23456]) # 내림 # 입력값과 같거나 작은 정수 중 가까운 값
temp_arr
# array([-2.,  1.])
```

```python
temp_arr = np.trunc([-1.23456, 1.23456])  # 버림
temp_arr
# array([-1.,  1.])
```
```python
temp_arr = np.fix([-1.23456, 1.23456]) # 가까운 정수로 반올림
temp_arr
# array([-1.,  1.])
```

```python
temp_arr = np.around([-1.23456, 1.23456], 2) # 반올림, 자릿수 지정 가능
temp_arr
# array([-1.23,  1.23])
```



