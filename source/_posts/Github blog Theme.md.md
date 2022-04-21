﻿---
title: "Hexo Blog 테마 설정 하기"
excerpt: "Hexo 기반 Blog에 테마를 설정하는 방법을 알아본다."
categories:
- Setting
tags:
- [Github blog]
- [Setting]
date: 2022-03-14 20:00:00
---
# hexo blog 테마 설정
[hexo 테마 공식 사이트](https://hexo.io/themes/)에 접속하여 원하는 테마를 찾아본 다음, 
테마의 공식 문서 설명에 따라 설치하면 된다.

## icarus 설정
터미널을 실행하고 하단의 코드를 입력한다
```
$ npm install --save hexo-theme-icarus
```
hexo blog 경로의 config.yml 파일을 실행한 뒤 theme를 fluid로 수정한다.
```
theme: icarus
```
서버를 실행하여 제대로 적용되었는지 확인한다.
```
hexo server
```
## fluid 설정
터미널을 실행하고 하단의 코드를 입력한다.
```
$ npm install --save hexo-theme-fluid
```
hexo blog 경로의 config.yml 파일을 실행한 뒤 theme를 fluid로 수정한다.
```
theme: fluid
language: zh-CN
```
터미널에 하단의 코드를 입력하여 about 페이지를 만든다.
```
$ hexo new page about
```
서버를 실행하여 제대로 적용되었는지 확인한다.
```
hexo server
```


