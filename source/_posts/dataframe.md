---
title: "데이터프레임에 대해 알아보자"
excerpt: "데이터프레임에 대해 알아보고 만들어본다 "
categories:

- R
tags:
- [R Study]
date: 2022-03-12

---
# 데이터프레임이란?
![데이터프레임의 기본 구조](https://user-images.githubusercontent.com/65166786/158939030-5efb7826-18cf-4501-97a0-23e86d14685e.PNG)

데이터프레임은 데이터 분석에서 가장 많이 사용되는 형태이다.

다양한 유형의 벡터들을 묶을 수 있는 다중형 구조이며, 행(row)과 열(column)로 구성되는 2차원 구조이다.


## 데이터프레임의 작성 방법

```r
객체(변수) <- data.frame(벡터1,벡터2..) 
```

```r
# 벡터를 별도로 만들기
sex <- c("male", "female")
korean <- c(87,92)
english <- c(88,95)

# 벡터들을 객체 exam_a에 입력

exam_a <- data.frame(sex,korean,english)
```
## 데이터의 위치로 만들기

```r
exam[2:4, 2:4] == exam [c(2,3,4),c(2,3,4)] 
```

```r
exam[c(-2,-4)] == exam [-c(2,4)] #특정열 제외하는 용도로 사용한다.
```

## 리스트 만들기

```r
b ← c(1,2)
c ← “hello”
d ← list(b,c) # b,c로 구성된 리스트 d 만들기
[[1]]  # 리스트의 첫번째를 출력 요청
[1] 1 2 # 리스트에 첫번째로 입력한 1 2 가 출력됨
[[2]]
[1] "hello" 
```











