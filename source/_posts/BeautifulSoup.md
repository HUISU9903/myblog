---
title: "BeautifulSoup"
excerpt: "BeautifulSoup의 설치방법과 기본적인 코드를 알아본다."
categories:
- Python
tags:
- [Python]
- [Beautifulsoup]
- [Web crawling]
date: 2022-04-21 18:00:00
---
# BeautifulSoup를 이용한 웹 크롤링
## BeautifulSoup 설치
beautifulsoup를 설치한다.
```
pip  install  beautifulsoup4
```
 `BeautifulSoup` 를 불러 온다.
 requests는 데이터를 전송할 때 딕셔너리 형태로 내보낸다.
만약 존재하지 않는 페이지를 요청해도 500,404등의 에러를 반환하지 않는다.
```
import requests 
from bs4 import BeautifulSoup 
```
## 기본 코드 알아보기
```
soup.title
```
웹 문서의 타이틀을 가져온다.
```
soup.p
```
웹 문서의 `<p>` 태그를 가져온다.
```
soup.find_all('p')
```
웹 문서 전체의`<p>`태그를 모두 출력한다.

```
soup.find(id="link3")
```
웹 문서에서 가장 처음으로 발견되는 `id="link3"`을 출력한다.

```
for link in soup.find_all('a'):
    print(link.get('href'))
```
웹 페이지 내의 `<a>` 태그 내에 있는 모든 URL을 추출 한다.

```
print(soup.get_text())
```
웹 페이지 내의 모든 텍스트를 추출 한다.
```
print(soup.prettify())
```
코드를 정리하여 출력한다.

