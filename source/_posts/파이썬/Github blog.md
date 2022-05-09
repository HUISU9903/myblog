---

title: "Hexo 블로그 만들기"
excerpt: "Hexo 블로그 만들기"
categories:

- other
tags:
- [other]
date: 2022-03-10 10:00:00

---

# Hexo란

Node.js기반의 블로그 생성기이며, 지킬(jekyll)에 비해 빠른 빌드와 반영 속도를 장점으로 가지고 있다.

## 파일 설치

* [node.js](https://nodejs.org/en/)  다운로드 (LTS로 다운하는것을 추천)

* [Git](git-scm.com) 다운로드 (운영체제에 맞게 다운로드)

* hexo 설치

터미널을 연 다음 코드를 입력하여 hexo를 설치한다.
```
$ npm install hexo-cli -g
```

hexo 블로그를 설치할 디렉토리로 이동하여 터미널에 코드를 입력한다. 
```
$ hexo init hexo
$ cd hexo
$ npm install
$ npm install --save hexo-deployer-git
```

설치 후 터미널에서 명령어를 실행해본다. 서버가 구동되면
http://localhost:4000  를 인터넷 브라우저에 입력하여 헥소가 정상적으로 작동하는지 확인한다.
```
$ hexo server
```


## Github 설정
Hexo 블로그는 두개의 깃허브 저장소를 사용하는데, 하나는 배포 페이지이고, 다른 하나는 실제 소스가 들어가는 페이지이다.

배포 페이지 저장소 이름은 사용자이름.github.io로 만들어야 기억하기 쉽다. 실제 소스가 저장되는 저장소도 하나 만든다.

[Github 저장소 만들기](https://github.com/new)


## config.yml 설정

Github를 통해 블로그를 웹에서도 접속되게 할려면 config.yml에서의 설정이 필요하다.
Hexo가 설치된 경로로 이동한 후, config.yml 파일을 연다.

```commandline
deploy:
  type: ''
```
아래 처럼 변경해준다.
```commandline
deploy:
  type: git
  repo: https://github.com/사용자계정/사용자계정.github.io.git
  branch: main
```
config.yml에서 사이트의 제목과 url도 수정할 수 있다.
```commandline
title: 제목을 지어주세요
subtitle: 부제목을 지어주세요
description: description을 지어주세요
author: YourName
```
```commandline
url: https://rain0430.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

## github에 배포하기

정적 리소스가 생성되고 배포를 진행한다.

```commandline
$ hexo deploy --generate
```

깃허브 저장소에 실제 소스를 저장하는 과정이다.

처음 실행시에만 (1),(2)를 함께 실행하고,이 후에는 (2)만 실행해도 된다.
(1)
```
$ git init
$ git remote add origin 저장소 주소
```
(2)
```
$ git add .
$ git commit -m '커밋 메세지'
$ git push origin master
```
git의 히스토리 충돌로 push가 안되는 경우에는 아래의 명령어를 사용하여 해결한다.

```commandline
$ git pull origin 브랜치명 --allow-unrelated-histories
```
