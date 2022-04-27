---
title : "Oracle 19c를 Windows에 설치하기"
excerpt: "Oracle 19c를 Windows에 설치하는 방법에 대해 알아본다."
categories:
- Setting
- Oracle
tags:
- [Oracle]
- [Setting]
date: 2022-04-23 18:00:00
---

﻿# Oracle19c의 Windows 설치

## Sql Developer의 설치

[Sql Developer](https://www.oracle.com/tools/downloads/sqldev-downloads.html)를 ZIP로 다운 받는다.
![image](https://user-images.githubusercontent.com/65166786/165004190-282f737c-ec6a-4f25-9417-459832731bd4.png)
압축을 풀면 설치가 끝난다.

## Oracle의 설치
[Oracle 다운로드 사이트](https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#19c)에 접속하여 Oracle 19c를 ZIP로 다운받는다.  

![image](https://user-images.githubusercontent.com/65166786/164979804-fab8fa2d-4908-470c-80fa-871fa92050b1.png)
압축을 푼다. 

C 드라이브에 Oracle 폴더를 만든 뒤 파일을 옮긴 후 설치 파일을 실행한다.
단일 인스턴스 데이터베이스 생성 및 구성 
데스크톱 클래스 > 가상 계정 사용
전역 데이터베이스 이름을 `myoracle`로 수정하고
컨테이너 데이터베이스로 생성에 체크해준다.
비밀번호는 쉬운 걸로 설정한다.(ex. 1234)
![image](https://user-images.githubusercontent.com/65166786/165495195-fe5113b8-270c-4d62-8b3e-3c6600c2ec3d.png)
그 후 설치를 누르면 설치된다.







