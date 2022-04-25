---
title : "Oracle의 데이터 타입"
excerpt: "Oracle에서 제공되는 데이터 타입과 그 활용법에 대해 알아본다"
categories:
- Oracle
tags:
- [Oracle]
- [SQL]
date: 2022-04-23 18:00:00
---
﻿# Oracle 데이터 타입
## 문자 데이터 타입
| 데이터 타입 | 설명 |
| --- | --- |
| CHAR (크기[ BYTE , CHAR ]) | 고정길이 문자, 최대 2000byte, default 값은 1byte |
| VARCARCHAR2 (크기[ BYTE , CHAR ]) | 가변길이 문자, 최대 4000byte, default  값은 1byte |
| NCHAR (크기)  | 고정길이 유니코드 문자(다국어 입력가능), 최대 2000byte, default  값은 1 |
| NVARCARCHAR2 (크기) | 가변길이 유니코드 문자(다국어 입력 가능), 최대 4000byte, default  값은 1 |
| LONG | 최대 2GB 크기의 가변길이 문자형, 잘 사용하지 않는다. |

고정길이의 경우 10byte라고 설정하면 `abc` 세 글자를 입력했을때 실제 크기가3byte이더라도  항상 10byte의 공간을 차지하게 되나,
`VARCARCHAR2(가변길이)`의 경우 똑같이 10byte라고 설정해도 `abc` 세 글자를 입력했을때 3byte의 공간만 차지하게 된다.

## CHAR & VARCHAR
고정길이(CHAR)와 가변길이(VARCHAR)로 이루어진 테이블을 생성한다.
```
CREATE TABLE ex2_1 (
    COLUMN1 CHAR(10),
    COLUMN2 VARCHAR(10)
);
```
`abc`를 고정길이(CHAR)와 가변길이(VARCHAR)에 입력한다.
```
INSERT INTO ex2_1 (column1, column2) VALUES ('abc', 'abc');
```
고정길이(CHAR)과 가변길이(VARCHAR)에 각각 `abc`를 입력했을때
고정길이는 10의 길이를 가지나, 가변길이는 `abc`의 길이는 3의 길이를 가지는 것을 확인할 수 있다.
```
SELECT column1
       , LENGTH(column1) as len1
       , column2
       , LENGTH(column2) as len2
 FROM ex2_1;
```
![고정길이와 가변길이](https://user-images.githubusercontent.com/65166786/165080984-167a59f7-6fda-43a5-92b1-711340149411.png)

## VARCHAR2
`NVARCHAR2`는 유니코드 문자형으로 다국어 입력이 가능하다.
```
CREATE TABLE ex2_2(
        COLUMN1    VARCHAR2(3), # default 값인 byte가 적용된다.
        COLUMN2    VARCHAR2(3 byte),
        COLUMN3    VARCHAR2(3 char)
);
```
```
INSERT INTO ex2_2 VALUES('abc', 'abc', 'abc');
```
```
SELECT column1, LENGTH(column1) AS len1
        ,column2, LENGTH(column2) AS len2
        ,column3, LENGTH(column3) AS len3
    FROM ex2_2;
```
영문자 `abc`의 경우 크기가 모두 3byte로 나오는 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/65166786/165047780-af36d7a9-dec9-4eab-bd71-3333bfbd1a16.png)

이번에는 영문자가 아닌 한글을 넣어본다. 영어는 문자 하나당 1byte의 공간을 가지지만, 한글의 경우 2byte의 공간을 차지한다.

```
INSERT INTO ex2_2 VALUES ('홍길동', '홍길동', '홍길동');
```
문자 하나당 2byte의 저장공간이기에 3byte의 공간으로 정의된 `column1,column2`에 들어갈 수 없어 오류가 발생하고 말았다.
```
INSERT INTO ex2_2 (column3) VALUES ('홍길동');
```
하지만 `column3`에서는 `byte`가 아닌 `char`을 명시했기에 입력이 가능하다.
```
SELECT column3, 
    LENGTH(column3) AS len3, 
    LENGTHB(column3) AS bytelen
FROM ex2_2;
```
![홍길동](https://user-images.githubusercontent.com/65166786/165048939-0f0bf2ba-37da-4ac7-a2d3-8d4fd1f9af63.png)
이전에 한글 한 문자는 2byte의 공간을 가진다고 했지만,
DB의 설정에 따라 3byte의 공간을 가지는 경우도 있다.

## 숫자 데이터 타입
| 데이터 타입 |  설명 |
| --- | --- |
| NUMBER [(p, [s])] | 가변숫자 , p(1~38, default 값은 38)와 s(-84~127, default 값은 0)는 십진수 기준, 최대 22byte |
| FLOAT[(p)] | NUMBER의 하위 타입. p는 1~128. default 값은 128. 이진수 기준. 최대 22byte |
| BINARY_FLOAT | 32비트 부동소수점 수. 최대 4byte |
| BINARY_DOUBLE | 64비트 부동소수점 수. 최대 8byte |

총 4가지의 숫자 타입 중 NUMBER만 사용할 때가 많다.
`INTEGER`같은 정수형, `DECIMAL`같은 실수형도 있지만,
모두 내부적으로는 `NUMBER`으로 변환되어 생성된다.
```
CREATE TABLE ex2_3(
    COL_INT INTEGER
    , COL_DEC DECIMAL
    , COL_NUM NUMBER
);
```
생성된 테이블 컬럼의 타입과 길이는 user_tab_cols라는 시스템 뷰를 조회하면 알 수 있다.
```
SELECT
    column_id
    ,column_name
    ,data_type
    ,data_length
    FROM user_tab_cols
WHERE table_name = 'EX2_3'
ORDER BY column_id;
```
모두 `NUMBER`형으로 생성되었고 크기는 22인것을 확인할 수 있다.
`NUMBER`형은 따로 크기를 명시하지 않으면 최대 크기인 22byte로 생성된다. 그리고 NUMBER (p, s) 형식으로 크기를 지정할 수도 있는데,
p(precision)는 최대 유효숫자 자릿수를 s(scale)는 소수점 기준 자릿수를 의미한다.
![image](https://user-images.githubusercontent.com/65166786/165050377-88f7e068-d246-460f-977d-f91f542bcf5d.png)
## FLOAT형으로 인한 오류
```
CREATE TABLE ex2_4 (
    COL_FLOT1 FLOAT(32) #p값을 32로 지정
    ,COL_FLOT2 FLOAT
);
```
```
INSERT INTO ex2_4 (col_flot1, col_flot2) VALUES (1234567891234, 1234567891234);
```
```
SELECT * FROM ex2_4;
```
![image](https://user-images.githubusercontent.com/65166786/165050609-225c3f79-a4ef-4958-bfc4-87d2b3275eef.png)
FLOAT(p)에서는 p에 들어가는 자릿수는 이진수를 기준으로 한다. 이진수 기준 32자리를 십진수로 변환 하려면 0.30103을 곱해야 한다. 32 * 030103 = 9.63296 이므로
1234567891234에서 열번째 자리만 제대로 들어가고
나머지는 0으로 들어가는 것을 확인 할 수 있다.

FLOAT 타입에서 p의 default값은 126(37.92978)이다.

## 날짜 데이터 타입
| 데이터 타입 |  설명 |
| --- | --- |
| DATE | BC 4712년 1월 1일부터 9999년 12월 31일, 연도,월,일,시,분,초까지 입력 가능하다. |
| TIMESTAMP [(fractional_seconds_precision)]   | 연도,월,일,시,분,초,밀리초까지 입력 가능하다. fractional_seconds_precision은 0~9까지 입력할 수 있고 default 값은 6이다. 
```
CREATE TABLE ex2_5(
    COL_DATE DATE
    ,COL_TIMESTAMP TIMESTAMP
);
```
`SYSDATE`와 `SYSTIMESTAMP`는 현재 일자와 시간을 반환하는 오라클 내부 함수 이다.
```
INSERT INTO ex2_5 VALUES (SYSDATE, SYSTIMESTAMP);
```
```
SELECT * FROM ex2_5;
```
![image](https://user-images.githubusercontent.com/65166786/165050763-00f05529-e157-44c3-b322-ae1fc8f43d22.png)
