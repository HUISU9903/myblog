---
title: "Hexo 블로그 만들기"
excerpt: "Github를 이용하여 Node.js 기반의 Hexo 블로그를 만든다."
categories:
- Setting
tags:
- [Github blog]
- [Setting]
date: 2022-03-10 10:00:00
---
# Hexo 블로그

Node.js기반의 블로그인 hexo 블로그를 설치하는 방법에 대해 배워본다.

##  프로그램 설치 및 환경 설정
hexo 블로그의 버전 관리를 위한 프로그램인 git을 먼저 설치한다.
```
sudo apt install git
```
그 후 Node.js를 설치한다.
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
```
sudo apt-get install nodejs
```
```
node --version
```
Ubuntu 20.04 버전의 경우 방화벽이 기본적으로 되어 있기에,
hexo server를 방화벽에서 허용하는 작업이 필요하다.
```shell
$ sudo apt install ufw
```
```shell
$ sudo ufw allow "OpenSSH"
```
```shell
$ sudo ufw enable
```
포트 4000의 경우 hexo server의 기본 포트이다.
```shell
$ sudo ufw allow 4000
```
```shell
$ sudo ufw allow http
$ sudo ufw allow https
```
## hexo 설치
hexo 패키지를 설치한다.
```
$ sudo npm install hexo-cli -g
```
hexo server가 설치될 폴더를 생성한다.
```
$ sudo mkdir -p /home/huisu/Documents/hexo
```
hexo server 폴더에 필요한 권한과 소유권을 지정한다.
```
$ sudo chown $USER:$USER /home/huisu/Documents/hexo
$ sudo chmod -R 755 /home/huisu/Documents/hexo
```
hexo server 폴더로 이동한다.
```
$ cd /home/huisu/Documents/hexo
```
hexo 블로그를 초기화 한다.
```shell
hexo init
```
hexo를 설치한다.
```shell
npm install
```
## Github 설정
hexo 블로그는 두개의 깃허브 저장소를 사용하는데, 하나는 배포 페이지이고, 다른 하나는 실제 소스가 들어가는 페이지이다.

배포 페이지 저장소 이름은 사용자이름.github.io로 만들어야 기억하기 쉽다. 실제 소스가 저장되는 저장소도 하나 만든다.

[Github 저장소 만들기](https://github.com/new)

## config.yml 설정

Github를 통해 블로그를 웹에서 접속되게 하려면 Github 연동이 필요하다.
config.yml에서 Github연동이 가능하다.
Hexo가 설치된 경로로 이동한 후, config.yml 파일을 연다.

Github 연동을 위해 하단 처럼 변경한다.
```
deploy:
  type: git
  repo: https://github.com/huisu9903/huisu9903.github.io.git
  branch: main
```
또한 title, subtitle, author 등을 수정하여 자신만의 블로그를 만들 수 있다.
```
title: 제목을 지어주세요
subtitle: 부제목을 지어주세요
description: description을 지어주세요
author: YourName
```
```
url: https://huisu9903.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

설정이 완료되면 hexo가 설치된 폴더에서 터미널을 연 다음 하단의 명령어를 입력한다.
```
$ hexo generate 
$ hexo server
```
 http://localhost:4000 를 인터넷 브라우저에 입력하여 헥소가 정상적으로 작동하는지 확인한다.

## github에 배포하기
하단의 명령어를 입력하여 배포를 진행한다.
```
$ hexo deploy --generate
```
배포가 완료되면 username.github.io로 접속해서 배포가 정상적으로 되었는지 확인해본다.


