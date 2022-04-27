﻿---
title : "Oracle 함수"
excerpt: "Oracle 함수"
categories:
- Oracle
tags:
- [Oracle]
- [Expression]
- [Condition]
date: 2022-04-27 22:00:00
---
# 함수
함수는 연산 대상과 특성에 따라 숫자 함수, 문자 함수, 날짜 함수, NULL 관련 함수, 변환 함수로 나눌 수 있다.
## 숫자 함수

### ABS
`ABS`는 절대값을 반환하는 함수이다.
`ABS(n)` 의 형태로 사용한다.
```
SELECT ABS(10) 
     , ABS(-10)
     , ABS(-10.123)
     FROM DUAL;
```
10 / 10 / 10.123 이 반환되었다.

### CEIL
`CEIL`은 올림한 결과를 반환한다.
`CEIL(n)`의 형태로 사용한다.
```
SELECT CEIL(10.123) 
     , CEIL(10.541)
     , CEIL(11.001)
     FROM DUAL;
  ```
 11 / 11 / 12가 반환되었다.
 
### FLOOR
`FLOOR`은 버림한 결과를 반환한다.
`FLOOR(n)`의 형태로 사용한다.
```
SELECT FLOOR(10.123)
      ,FLOOR(10.541)
      ,FLOOR(11.001)
      FROM DUAL;
```
10 / 10 / 11이 반환되었다.

### ROUND
`ROUND`는 반올림한 결과를 반환한다. 
`ROUND(n,i)`로 사용하며, `i`를 통해 반올림 할 자릿수의 지정이 가능하다.
```
SELECT ROUND(10.154)
             ,ROUND(10.541)
             ,ROUND(11.001)
             ,ROUND(10.154, 1)
             ,ROUND(10.154, 2)
             ,ROUND(10.154, 3)
             FROM DUAL;
```
10 / 11/ 11 / 10.2 / 10.15 / 10.154 가 출력되었다.
```
SELECT ROUND(0, 3)
      ,ROUND(115.155, -1)
      ,ROUND(115.155, -2)
      FROM DUAL;
```
반올림할 자릿수를 음수로 지정하면 자릿수를 왼쪽부터 카운팅한다.
0 / 120 / 100이 출력되었다.

### TRUNC
`TRUNC`는 반올림을 하지 않고 자릿수를 잘라낸 뒤 반환한다.
`TRUNC(n,i)`로 사용하며, `i`에 자릿수를 지정하면 그 자릿수에서 잘라낸 결과를 반환한다. 
```
SELECT TRUNC(115.155)
      ,TRUNC(115.155, 1)
      ,TRUNC(115.155, 2)
      ,TRUNC(115.155, -2)
FROM DUAL;
```
115 / 115.1 / 115.15 / 100 이 반환되었다.

### POWER
`POWER`는 n1을 n2 제곱한 결과를 반환한다.
n1이 음수 일때 n2는 정수만 올 수 있다.
`POWER(n1,n2)`로 사용한다.
```
SELECT POWER(2,3), POWER(3,3)
 FROM DUAL;
 ```
 8 / 27이 반환되었다. 
 ```
SELECT POWER(-3, 3.0001)
 FROM DUAL;
 ```
 ![image](https://user-images.githubusercontent.com/65166786/165432532-5048ecde-297f-4dba-9813-65da9cc215bb.png)
n1이 음수일 때 n2를 정수가 아닌 실수로 할 경우 
"인수가 범위를 벗어났습니다" 라는 에러가 발생한다.

### SQRT
`SQRT`는 n의 제곱근을 반환한다.
`SQRT(n)`으로 사용한다.
```
SELECT SQRT(2), SQRT(5)
 FROM DUAL;
```
1.73205 / 2.64575 이 반환되었다.

### MOD 
`MOD`는 n1을 n2로 나눈 나머지 값을 반환한다.
`MOD(n1,n2)`로 사용한다.
```
SELECT MOD(17,3), MOD(21.123, 5.2)
 FROM DUAL;
 ```
 2 / 0.323 이 반환되었다.
 
### REMAINDER  
`REMAINDER` 또한 나머지 값을 반환한다.
`MOD`와는 차이점이 있다. 
`REMAINDER(n1,n2)`로 사용한다.

 `MOD`의 경우 n1 - n2 * FLOOR(n1/n2)로 `n1/n2`에 대해 버림을 시행하지만,
`REMAINDER`의 경우 n1 - n2 * ROUND(n1/n2)로 `n1/n2`에 대해 반올림을 시행하는 차이점이 있다.
  ```
 SELECT REMAINDER(17,3), REMAINDER(19.123, 4.2)
 FROM DUAL;
 ```
 값이 반올림되어 -1 / 0.323이 반환되었다.
 
## 문자 함수

### INITCAP 
`INITCAP`는 문자의 첫 문자는 대문자, 나머지는 소문자로 반환하는 함수이다. 첫 문자의 기준은 공백이나 알파벳이 아닌 문자이다.
`INITCAP(char)`로 사용한다.
```
SELECT INITCAP('good moring today'), INITCAP('good뷁moring*today')
 FROM DUAL;
 ```
 ![image](https://user-images.githubusercontent.com/65166786/165435002-d7fbe300-e17d-4c2d-b127-411ad3b8986c.png)
 왼쪽 출력을 보면 공백을 기준으로 `G``M``T`가 대문자로 변경된 것을 확인 할 수가 있고, 오른쪽 출력을 보면 한글 `뷁`과 `*`을 기준으로 대문자로 변경된 것을 확인 할 수 있다.
### LOWER & UPPER
`LOWER`는 문자를 모두 소문자로, `UPPER`는 문자를 모두 대문자로 변환한다.
`LOWER(char), UPPER(char)`로 사용한다.
```
SELECT LOWER('GOOD MORING TODAY'), UPPER('good moring today')
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165435910-276955af-4678-459a-9833-63bc252027e1.png)
왼쪽 출력을 보면 모두 소문자로 변환된 것을 확인 할 수 있고, 오른쪽 출력을 보면 대문자로 변환된 것도 확인 할 수 있다.
### CONCAT
`CONCAT`은 두 문자를 붙여서 반환한다.
`CONCAT(char1, char2)`로 사용한다.
```
SELECT CONCAT('I Want', ' Go Home'), 'I Want' || ' Go Home'
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165436028-0db99d38-cafa-44b0-ab67-c9c5729294ef.png)
문자 연산자인 `||`과 동일한 역할을 한다는 것을 알 수 있다. 

### SUBSTR & SUBSTRB
`SUBSTR`은 문자열의 `pos`번째 문자부터 `len`길이만큼 잘라낸 결과를 반환한다. `pos`값이 0일 경우 첫번째 문자를 가르키며 , 음수일 경우 문자열의 맨 끝부터 역순으로 카운팅한다. 또한 `len`값이 생략될 경우 `pos`번째부터 나머지 모든 문자를 반환한다.
`SUBSTR(char, pos, len)`, `SUBSTRB(char, pos, len)`으로 사용한다. 
```
SELECT SUBSTR('ABCDEFG', 1, 4), SUBSTR('ABCDEFG', -1, 4)
 FROM DUAL;
 ```
![image](https://user-images.githubusercontent.com/65166786/165444255-5c46995c-6a20-4899-abb9-8aead34b1db9.png)
왼쪽 출력을 보면 첫번째 문자(1)에서 길이 4까지 반환하기에 `ABCD`가 반환되었다.
오른쪽 출력을 보면 역순으로 뒤에서 첫번째 문자에서(-1) 길이 4를 반환하나, 문자가 하나밖에 없기에 `G`만 반환된다.

`SUBSTR`은 문자의 개수로 문자열을 자르지만,
`SUBSTRB`는 문자 개수가 아닌 문자열의 Byte를 기준으로
문자열을 잘라낸다.
```
SELECT SUBSTRB ('ABCDEFG', 1, 4), SUBSTRB('가나다라마바사', 1, 4)
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165445328-484f45ea-7040-4288-b16d-14b0575aa6be.png)
왼쪽 출력을 보면 첫번째 문자(1)에서 4byte만 반환하기에 `ABCD`가 반환되었다.
오른쪽 출력을 보면 첫번째 문자(1)에서 4byte만 반환하기에 `가나`가 출력되어야 하지만(한글은 통상적으로 한 글자당 2byte이다.) Oracle 인코딩에 따라 한글 한 문자 당 3byte의 저장공간을 가지게 될수도 있다고 한다. 문자 하나당3byte의 저장공간을 가지게 되어서 `가`만 출력되었다.

### LTRIM & RTRIM
`LTRIM`은 문자열에서 `set`로 지정된 문자열을 왼쪽 끝에서 제거한 후 나머지 문자열을 반환한다.
`set`를 생략할 경우 기본값으로 공백 한 문자가 사용된다.
`RTRIM`은 `LTRIM`과 반대로 오른쪽 끝에서 제거한 후 나머지 문자열을 반환한다.
`LTRIM(char, set)` , `RTRIM(char, set)`으로 사용한다.
```
SELECT LTRIM('QWERTYGOODQWERTY', 'QWERTY')
      ,LTRIM('기러기', '기')
      ,RTRIM('QWERTYGOODQWERTY', 'QWERTY')
      ,RTRIM('기러기', '기')
 FROM DUAL;
 ```
![image](https://user-images.githubusercontent.com/65166786/165447162-b1a76df0-6c15-45d0-9aeb-eef07cb66eba.png)

### LPAD & RPAD
`LPAD`,`RPAD`는 매개변수로 들어온 문자열을 n자리만큼 왼쪽/ 오른쪽부터 채운 후 반환한다. 
`LPAD(expr1,n,expr2)` , `RPAD(expr1,n,expr2)`로 사용한다.
```
CREATE TABLE ex4_1 (
 alpha VARCHAR2(30)
);
INSERT INTO ex4_1 VALUES ('ABC');
INSERT INTO ex4_1 VALUES ('DEF');
INSERT INTO ex4_1 VALUES ('GHI');
```
```
SELECT LPAD(alpha, 7, 'Play')
 FROM ex4_1;
```
![image](https://user-images.githubusercontent.com/65166786/165467899-6d7f65a2-90f2-44f2-9bc9-17c14f90ef93.png)
```
SELECT RPAD(alpha, 7, 'Play')
 FROM ex4_1;
```
![image](https://user-images.githubusercontent.com/65166786/165467939-67118faa-ed3e-4b01-8e6c-64bf7d158d3e.png)

### REPLACE
`REPLACE`는 `char`에서 `search_str`을 찾아 `replace_str`로 대체한 결과를 반환한다. 
`REPLACE(char,search_str,replace_str)`으로 사용한다.
```
SELECT REPLACE ('얼음 공주같은 눈빛은 말고 한번쯤은 웃어도봐요 오금저리고 얼어붙어', '얼음 공주', '당근 공주')
 FROM DUAL;
 ```
![image](https://user-images.githubusercontent.com/65166786/165470648-0d952325-e560-4a4f-8e75-106f36d83306.png)
 `얼음 공주`가 `당근 공주`로 대체된 것을 확인 할 수 있다.
```
SELECT LTRIM(' ABC DEF ')
       ,RTRIM(' ABC DEF ')
       ,REPLACE(' ABC DEF ', ' ', '')
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165471158-4f2e4cd2-fd28-4f4a-8786-423c43b35da6.png)
`LTRIM`, `RTRIM`의 경우 문자열 중간의 공백이 제거되지 않지만, `REPLACE`의 경우 문자열 중간의 공백도 제거된 채로 출력되는 것을 확인 할 수 있다.
### TRANSLATE
`TRANSLATE`는 `expr`에서 `from_str`을 찾아 `to_str`로 대체한 결과를 반환한다. 
`TRANSLATE(expr,from_str,to_str)`으로 사용한다.
```
SELECT REPLACE('얼음 공주같은 눈빛은 말고 한번쯤은 웃어도봐요 오금저리고 얼어붙어', '얼음 공주', '당근 공주') AS rep
      ,TRANSLATE('얼음 공주같은 눈빛은 말고 한번쯤은 웃어도봐요 오금저리고 얼어붙어', '얼음 공주', '당근 공주') AS trn
 FROM DUAL;
 ```

![image](https://user-images.githubusercontent.com/65166786/165472988-152cfa62-e65f-47e6-97d9-7efb4d9ceb32.png)
`REPLACE`의 경우 단어 단위로 바뀌지만, `TRANSLATE`는 한 글자씩 바뀌기에
오른쪽 출력을 보면 '얼어붙어'가 아닌 '당어붙어'가 된 것을 확인 할 수 있다.

### INSTR
`INSTR`은 str 문자열에서 substr과 일치하는 위치를 반환한다.
`INSTR(str,substr,pos,occur)`으로 사용한다.
`pos`는 시작위치이고, `occur`은 몇 번째 일치하는지를 명시한다.
```
SELECT INSTR('내가 만약 외로울 때면. 내가 만약 괴로울 때면. 내가 만약 즐거울때면', '만약') AS INSTR1
      ,INSTR('내가 만약 외로울 때면. 내가 만약 괴로울 때면. 내가 만약 즐거울때면', '만약', 5) AS INSTR2
      ,INSTR('내가 만약 외로울 때면. 내가 만약 괴로울 때면. 내가 만약 즐거울때면', '만약', 5, 2) AS INSTR3
 FROM DUAL;
 ```
![image](https://user-images.githubusercontent.com/65166786/165476835-486577c9-9d22-46e5-9679-c67fc9d2c133.png)
첫번째는 `pos`, `occur`둘 다 없으므로 기본값 1이 적용되어, 첫번째 만약이 있는 위치인 4가 반환되었다.
두번째는 `pos`에 5가 적용되어 5번째 글자부터 탐색하므로,
첫번째 만약을 건너뛴다. 그러므로 두번째 만약이 있는 위치인 18이 반환되었다.
세번째는 `pos`에 5, `occur`에 2를 적용했으므로 첫번째 만약을 건너뛰고, 두번째 만약도 건너뛴 세번째 만약의 위치인 32가 반환되었다.

### LENGTH & LENGTHB
`LENGTH`은 문자열의 개수를 반환한다.
`LENGTHB`는 문자열의 바이트수를 반환한다.
`LENGTH(chr)` , `LENGTHB(chr)`으로 사용한다.
```
SELECT LENGTH('대한민국')
      ,LENGTHB('대한민국')
 FROM DUAL;
 ```
 ![image](https://user-images.githubusercontent.com/65166786/165477950-8e44dffa-3a7f-4bc9-b30b-1d27bca721fe.png)
 `LENGTH`에서 문자열의 개수 4가 반환되었고,
 `LENGTHB`에서 문자열의 바이트 수 12가 반환된 것을 확인할 수 있다.
## 날짜 함수
날짜 함수는 날짜형을 대상으로 연산을 수행하는 함수이다.
### SYSDATE , SYSTIMESTAMP
`SYSDATE`, `SYSTIMESTAMP`는 현재일자와 시간을 각각 `DATE` `TIMESTAMP`형으로 반환한다.
```
SELECT SYSDATE, SYSTIMESTAMP
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165512183-fa129de6-8982-4030-bad8-749ee8234011.png)

### ADD_MONTHS
`ADD_MONTHS`는 날짜에 `integer` 만큼의 월을 더한 뒤 반환한다.
`ADD_MONTHS(date, integer)`으로 사용한다.
```
SELECT ADD_MONTHS(SYSDATE, 1), ADD_MONTHS(SYSDATE, -1)
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165512266-809fa569-45f0-4079-844a-409be2833545.png)
### MONTHS_BETWEEN
`MONTHS_BETWEEN`는 두 날짜 사이의 개월 수를 반환한다.
`MONTHS_BETWEEN(date1, date2)`로 사용하며, `date1`이 `date2`보다 빠른 날짜여야 한다.
```
SELECT MONTHS_BETWEEN(SYSDATE, ADD_MONTHS(SYSDATE, 1)) mon1,
       MONTHS_BETWEEN(ADD_MONTHS(SYSDATE, 1), SYSDATE) mon2
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165513978-1003fb14-b41a-4f74-8e1d-dfd449e60fd6.png)


### LAST_DAY
`date` 날짜를 기준으로 해당 월을 마지막 일자를 반환한다.
`LAST_DAY(date)`으로 사용한다.
```
SELECT LAST_DAY(SYSDATE)
      FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165514793-1304a135-463a-4690-a522-9f8dfa031b71.png)
### ROUND & TRUNC
`ROUND`와 `TRUNC`는 숫자 함수이면서 날짜 함수로도 쓰인다. `ROUND`는 반올림 날짜를, `TRUNC`는 잘라낸 날짜를 반환한다.
`ROUND(data,format)`, `TRUNC(data,format)`로 사용한다.
![image](https://user-images.githubusercontent.com/65166786/165520893-6f10d9ac-402d-4264-a120-9eba126e917f.png)

### NEXT_DAY
`data`를 `char`에 명시한 날짜로 다음주 주중 일자를 반환한다.
`NEXT_DAY(char)`으로 사용한다.
`char`에 오는 요일 값은 사용자 환경에 따라 한글이 될 수도 있고, 영어도 될 수도 있다.
```
SELECT NEXT_DAY(SYSDATE, '금요일')
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165521492-54c57b88-af70-4360-8690-fe3371480189.png)
## 변환 함수
서로 다른 유형의 데이터 타입으로 변환해서 결과를 반환하는 함수이다.
변환 함수를 통해 형변환을 직접 처리하는 것을 `명시적 형변환`이라고 한다.
반대로 일정한 규칙에 따라 자동으로 형변환이 처리되는것은 `묵시적 형변환`이라고 한다.
* 날짜 변환 형식

| 포맷  |  설명  |  예시 |
| --- | --- | --- |
| AM, A.M. | 오전 | TO_CHAR(SYSDATE, ‘AM’) → 오전 |
| PM, P.M. | 오후 | TO_CHAR(SYSDATE, ‘PM’) → 오후 |
| YYYY, YYY, YY, Y | 연도 | TO_CHAR(SYSDATE, ‘YYYY’) → 2014 |
| MONTH, MON | 월 | TO_CHAR(SYSDATE, ‘MONTH’) → 2월 |
| MM | 01~12 형태의 월 | TO_CHAR(SYSDATE, ‘MM’) → 02 |
| D | 주 중의 일을 1~7로 | TO_CHAR(SYSDATE, ‘D’) → 2 |
| DAY | 주 중 일을 요일로 표시 | TO_CHAR(SYSDATE, ‘DAY’) → 월요일 |
| DD | 일을 01~31 형태로 표시 | TO_CHAR(SYSDATE, ‘DD’) → 01 |
| DDD | 일을 001~365 형태로 | TO_CHAR(SYSDATE, ‘DDD’) → 041 |
| DL | 현재 날짜를 요일까지 표시 | TO_CHAR(SYSDATE, ‘DL’) → 2014년 2월 10일 월요일 |
| HH, HH12 | 시간을 01~12시 형태로 | TO_CHAR(SYSDATE, ‘HH’) → 04 |
| HH24 | 시간을 01~23시 형태로 | TO_CHAR(SYSDATE, ‘HH24’) → 16 |
| MI | 분을 00~59분 형태로 | TO_CHAR(SYSDATE, ‘MI’) → 56 |
| SS | 초를 00~59초 형태로 | TO_CHAR(SYSDATE, ‘SS’) → 33 |
| WW | 주를 01~52주 형태로 | TO_CHAR(SYSDATE, ‘WW’) → 06 |

* 숫자 변환 형식

| 포맷 | 설명 | 예시 |
| --- | --- | --- |
| , | 콤마로 표시 | TO_CHAR(123456, ‘999,999’) → 123,456 |
| . | 소수점 표시 | TO_CHAR(123456.4, ‘999,999.9’) → 123,456.4 |
| 9 | 한 자리 숫자, 실제 값보다 크거나 같게 명시 | TO_CHAR(123456, ‘999,999’) → 123,456 |
| PR | 음수일 때 < >로 표시 | TO_CHAR(-123, ‘999PR’) → <123> |
| RN, rn | 로마 숫자로 표시 | TO_CHAR(123, ‘RN’)→CXXIII |
| S | 양수이면 +, 음수이면 - 표시 | TO_CHAR(123, ‘S999’) → +123 |

### TO_CHAR
숫자나 날짜를 문자로 변환한다.
`TO_CHAR(숫자 혹은 날짜, format) `으로 사용한다.
```
SELECT TO_CHAR(123456789, '999,999,999')
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165528777-7db99cea-a14d-4cc8-a600-bd4eafe8e00c.png)
### TO_NUMBER
문자나 다른 유형의 숫자를 `NUMBER` 형태로 변환한다.
`TO_NUMBER(expr,format)`으로 사용한다.
```
SELECT TO_NUMBER('123456')
 FROM DUAL;
 ```
![image](https://user-images.githubusercontent.com/65166786/165530030-4d0fc429-ce40-4b33-877d-0df8e34f8881.png)
### TO_DATE & TO_TIMESTAMP
문자를 날짜형으로 변환한다. 
`TO_DATE(char, format)` , `TO_TIMESTAMP(char, format)`으로 사용한다. 
```
SELECT TO_DATE('20140101', 'YYYY-MM-DD')
 FROM DUAL;
SELECT TO_TIMESTAMP('20140101 13:44:50', 'YYYY-MM-DD HH24:MI:SS')
 FROM DUAL;
```
![image](https://user-images.githubusercontent.com/65166786/165532935-718227d3-6326-48c9-93fd-b3f05617a35d.png)
![image](https://user-images.githubusercontent.com/65166786/165532998-d7e2b0f6-7f6f-4678-ad6f-a4e405e9d68a.png)

`TIMESTAMP`를 실행 했을 때 시간이 표시되지 않으면 실행한다.
```
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD HH24:MI:SS';
```
## NULL 관련 함수
Oracle에서는 NULL을 연산 대상으로 처리할 수 있는 SQL 함수를 제공하고 있다.
### NVL & NVL2
`expr1`이 `NULL`일 때 `expr2`를 반환한다.
`NVL(expr1,expr2)`으로 사용한다. 
```
SELECT NVL(manager_id, employee_id)
 FROM employees
WHERE manager_id IS NULL;
```
`NVL2`는 `NVL`의 업그레이드로 `NVL2(expr1, expr2, expr3 ...)`으로 사용한다.
```
SELECT employee_id
     ,NVL2(commission_pct, salary +(salary * commission_pct), salary) AS salary2
 FROM employees;
 ```
### COALESCE
`COALESCE`는 매개변수로 들어오는 표현식에서 NULL이 아닌 첫 번째 표현식을 반환한다.
`COALESCE(expr1, expr2..)`의 형태로 사용한다.
```
SELECT employee_id, salary, commission_pct
       ,COALESCE(salary * commission_pct, salary) AS salary2
 FROM employees;
 ```
### LNNVL
`LNNVL`은 매개변수로 들어오는 조건식의 결과가 FASLE나 UNKNOWN이면 TRUE를, TRUE이면 FALSE를 반환한다.
`LNNVL(조건식)`의 형태로 사용한다.
```
SELECT COUNT(*)
 FROM employees
WHERE LNNVL(commission_pct>= 0.2);
```

### NULLIF 
`NULLIF`는 `expr1`과 `expr2`를 비교해 같으면 `NULL`을 다르면 `expr1`을 반환한다.
`NULLIF(expr1, expr2)`의 형태로 사용한다.
```
SELECT employee_id
    ,TO_CHAR(start_date, 'YYYY') start_year
      ,TO_CHAR(end_date, 'YYYY') end_year
      ,NULLIF(TO_CHAR(end_date, 'YYYY'), TO_CHAR(start_date, 'YYYY') ) nullif_year
 FROM job_history;
 ```
## 기타 함수

### GREATEST & LEAST
`GREATEST`는 매개변수로 들어오는 표현식에서 가장 큰 값을, `LEAST`는 가장 작은 값을 반환한다. 숫자뿐만 아니라 문자의 형태도 비교 가능하다.
```
SELECT GREATEST(1, 2, 3, 2)
      ,LEAST(1, 2, 3, 2)
FROM DUAL;
```
### DECODE
`DECODE`는 `expr`과 `search1`을 비교해 두 값이 같으면 `result1`을 다르면 `search2`와 비교해서 `result2`를 이런식으로 비교하다가 최종적으로 같은 값이 없을 경우 `defalut` 값을 반환한다. if문 중 elif문을 생각하면 이해가 쉬울 듯 하다.
`DECODE(expr, search1,result1,search2,result2....,default`)의 형태로 사용한다.
```
SELECT prod_id
      ,DECODE(channel_id, 3, 'Direct',
                          9, 'Direct',
                          5, 'InDirect',
                          4, 'InDirect',
                             'Others') decodes
 FROM sales
WHERE prod_id = 125;
```

