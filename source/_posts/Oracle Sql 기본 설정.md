---
title : "Oracle SQL 기본 설정"
excerpt: "sqlplus와 sql Developer의 기본적인 설정을 알아본다. "
categories:
- Oracle
tags:
- [Oracle]
- [SQL]
date: 2022-04-23 18:00:00
---

﻿# Sqlplus 설정

터미널을 연 다음 `Sqlplus`를 입력하여 `Sqlplus`에 로그인한다.
![sqlplus](https://user-images.githubusercontent.com/65166786/165076782-c46eaa26-6840-44b5-b0cf-1c431e6a2f1e.png)

## 테이블 스페이스 생성
```
CREATE TABLESPACE myts DATAFILE 'C:\sqldevelop\oradata\MYORACLE\myts.dbf' SIZE 100M AUTOEXTEND ON NEXT 5M;
```

![image](https://user-images.githubusercontent.com/65166786/165011283-6f6c40ca-acb3-4120-a438-e929683ea9fb.png)

## 사용자 계정 생성 
```
CREATE USER ora_user IDENTIFIED BY huisu DEFAULT TABLESPACE myts TEMPORARY TABLESPACE TEMP;
```
![image](https://user-images.githubusercontent.com/65166786/165011294-d1c78ae3-8a1e-49a3-a9a8-5902c6763f01.png)

오류 발생시 하단의 코드를 입력해준다.
```
alter session set "_ORACLE_SCRIPT"=true;
```
![image](https://user-images.githubusercontent.com/65166786/165078122-4963451e-5ec2-4977-b405-b3e5684565fc.png)
사용자 생성이 정상적으로 된 것을 확인 할 수 있다.
(myts인 테이블스페이스 이름을 myte로 오타를 냈다.)

`ora_user`에  DBA 권한을 부여한다.
```
GRANT DBA TO ora_user;
```
데이터 베이스에 접속한다.
```
connect ora_user/huisu;
```
![image](https://user-images.githubusercontent.com/65166786/165011687-11dac4c4-7202-4716-b37f-49916d9e45ae.png)


# Sql Developer 설정
Sql Developer를 실행한다. 
다른 버전의 환경설정을 불러오는지 물어보는 팝업이 뜨면 아니오를 누른다. 

## 새로운 연결 정보 설정하기
오른쪽 위 초록색 + 아이콘을 눌러 새로운 데이터베이스 접속을 만든다.
열리는 팝업창에서 설정을 진행한다.
사용자 이름 : ora_user
비밀번호 : huisu (IDENTIFIED BY 뒤의 단어)
SID : myoracle
![image](https://user-images.githubusercontent.com/65166786/165080147-fff07e91-1ab1-4b75-9b39-7ea4f1a9a35e.png)
설정한 뒤 테스트를 눌러 접속이 제대로 되는지 확인한다.
성공이라고 표시되면 접속을 누른다.
## 날짜 및 시간 형식 변경하기
도구 > 환경 설정 > 데이터 베이스 > NLS 항목으로 진입해서
날짜 형식을 `YYYY/MM/DD`로, 시간 형식을`YYYY/MM/DD HH24:MI:SS`로 변환한다.
![image](https://user-images.githubusercontent.com/65166786/165016347-a0e2cd1e-af34-488d-ab60-927db9530d41.png)

## 샘플 스키마 설치하기
샘플 스키마 파일을 다운 받는다.
```
$ git clone https://github.com/gilbutITbook/006696
```
C 드라이브에 backup 폴더를 생성한다.
터미널을 연 뒤 C:\backup 폴더로 이동한다. 
```
cd backup
```
스키마 파일을 설치한다.
`huisu`의 경우 비밀번호이므로
각자 설정한 비밀번호로 바꾸어야한다.
```
imp ora_user/huisu file=expcust.dmp log=expcust.log ignore=y grants=y rows=y indexes=y full=y
```
```
imp ora_user/huisu file=expall.dmp log=empall.log ignore=y grants=y rows=y indexes=y full=y
```
워크시트에 아래의 코드를 작성하여
10개의 행이 출력되는지 확인해본다.
```
SELECT table_name FROM user_tables;
```
![image](https://user-images.githubusercontent.com/65166786/165017760-1e1ce996-d65e-4724-8805-a41b7e096148.png)
제대로 출력되는 것을 확인할 수 있다.
