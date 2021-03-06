---
title: "데이터를 불러오는 방법"
excerpt: "데이터를 불러오는 방법을 알아본다 "
categories:

- R
tags:
- [R Study]
date: 2022-03-13 11:10:00

---
# 데이터를 불러오는 방법
R을 이용한 데이터 분석에서 가장 중요한 것은 데이터를 올바르게 불러오는 것이라고 생각한다.
이번에는 데이터 유형에 따라 다른 데이터를 불러오는 방법을 알아본다.

## 주로 쓰이는 데이터 유형

데이터 분석에서 주로 쓰이는 데이터 유형에는 .csv , .sav, .xlsx 등이 있다.
이 3가지 유형에 대해 알아보자.

## csv파일을 불러오는 방법
 ```r
객체 <- read.csv("데이터세트.csv")
```

3가지의 데이터 유형 중에 가장 간단한 방법이다.

## spss 파일을 불러오는 방법 

```r
install.packages("foreign")
library(foreign)

read.spss("객체.sav")
```
spss 데이터 분석에서 많이 쓰이는 프로그램으로 .sav 확장자의 파일을 생성한다.   
R studio의 내장함수로는 .sav를 불러올 수 없기 때문에 foreign 패키지의 도움이 필요하다.

## 엑셀파일을 불러오는 방법

```r
install.package("readxl")
iibrary(readxl)
read_excel("데이터세트.xlsx")
```
```r
install.package("readxl")
iibrary(readxl)
read_excel("데이터세트.xlsx",sheet = 2) # 두번째 시트 불러오기
```
내장함수를 통해 불러올 수 없기 때문에 readxl 패키지의 도움이 필요하다.  
여러개의 시트가 존재하는 파일의 경우 sheet ="숫자"를 통해 원하는 시트를 불러올 수 있다.

## 파일을 불러오는 옵션
```r
 stringsAsFactors = F # 문자를 범주로 만들지 말 것
```

```r
na = " " , na = "-"   # 결측치로 만들어서 불러올 것
```

## 마지막으로.. 
```r
getwd() # 현재 작업중인 워킹 디렉터리 확인
setwd() # 현재 폴더로 워킹 디렉터리 변경
```

파일이 내가 원하는 폴더에 없는 경우가 있다. 그런 경우 getwd()를 통해 현재 워킹 디렉터리를 확인하고,  
setwd()를 통해 내가 원하는 파일이 있는 폴더로 워킹 디렉터리를 변경해주면 해결된다.
