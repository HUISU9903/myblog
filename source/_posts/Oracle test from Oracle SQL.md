﻿---
title : "Oracle 함수 예제"
excerpt: "Oracle의 함수 예제 문제"
categories:
- Oracle
tags:
- [test]
- [Oracle]
date: 2022-04-29 13:00:00
---
# 6장
## 1번
질문 : 
101번 사원에 대해 
`사번` `사원명` `job명칭` `job시작일자` `job종료일자` `job수행부서명`을 산출하는 쿼리를 작성해보자.

답 : 
```
SELECT a.employee_id, a.emp_name, d.job_title, b.start_date, b.end_date, c.department_name
  FROM employees a
       ,job_history b
       ,departments c
       ,jobs d
 WHERE a.employee_id   = b.employee_id
   and b.department_id = c.department_id
   and b.job_id        = d.job_id
   and a.employee_id = 101;  
   ```
  `SELECT` 문에 `employee_id`, `emp_name`, `job_title`, `start_date`, `end_date`, `department_name`을 넣어주고 `WHERE`문에  `조인 연결`과 101번 사원을 명시해주면 된다.
  
   ## 2번
 질문 :
하단의 쿼리를 실행하면 오류가 발생한다. 
그 이유는 무엇인가?
   ```
SELECT a.employee_id, a.emp_name, b.job_id, b.department_id
      FROM employees a,
           job_history b
     WHERE a.employee_id      = b.employee_id(+)
       and a.department_id(+) = b.department_id;
```

답 : 
`(+)`연산자의 경우 조인 대상 테이블 중 데이터가 없는 테이블에만 붙일 수가 있는데 
```
SELECT department_id FROM job_history;
SELECT department_id FROM employees;
```
으로 `employees`와 `job_history`의 `department_id`를 조회해보면 `employees`의 데이터가 더 많다는 것을 알 수 있다. 그러므로  `employees`가 아닌 `job_history`에 `(+)`를 붙여야 오류가 발생하지 않는다.
## 3번 
질문 : 
외부 조인을 사용할 때에 `(+)`연산자를 같이 사용할 수 없으나, `IN` 절에 사용하는 값이 한 개이면 사용이 가능하다.
그 이유는 무엇인가

답 : `(+)` 연산자와 `OR`은 함께 사용할 수 없다.
그러므로 `IN`절에 사용하는 값이 여러 개이면 `OR` 조건으로 들어가게 되기에 `IN`절 또한 사용할 수 없다.
하지만 `IN`절에 하나의 값만 있을 경우에는 `OR`조건이 아니기에 사용이 가능하다.
 
## 4번 
질문 :  다음의 쿼리를 ANSI 문법으로 변경해 보자.
```
SELECT a.department_id, a.department_name
      FROM departments a, employees b
     WHERE a.department_id = b.department_id
       AND b.salary > 3000
    ORDER BY a.department_name;
 ```
 답 : ANSI 문법으로 변경하라는 것은 ANSI 조인으로 변경하라는 뜻이다.
 ```
 SELECT a.department_id, a.department_name
 FROM departments a
      INNER JOIN employees b
      ON (a.department_id = b.department_id)
      WHERE b.salary > 3000
      ORDER BY a.department_name;
  ```
`FROM`절에서 INNER JOIN을 쓴 다음, 기존에 `WHERE`절에 썼던 조인 조건을 `ON`절에 써준다. 나머지 다른 조건의 경우 기존대로 `WHERE`절에 써준다.
## 5번
질문 : 다음은 연관성 있는 서브 쿼리이다. 이를 연관성 없는 서브 쿼리로 변환해 보자
```
 SELECT a.department_id, a.department_name
     FROM departments a
    WHERE EXISTS ( SELECT 1
                   FROM job_history b
                   WHERE a.department_id = b.department_id );
```
답 : 
```
SELECT a.department_id, a.department_name
 FROM departments a
WHERE a.department_id IN ( SELECT department_id
                            FROM job_history  );
 ```
 서브 쿼리는 `메인 쿼리와의 연관성`에 따라, 연관성 있는 서브 쿼리와 없는 서브 쿼리로 나뉜다. 연관성 없는 서브 쿼리는 `조인 조건`을 찾아 볼 수 없는 것이 특징이다.
## 6번
질문 :  연도별 이탈리아 최대매출액과 사원을 작성하는 쿼리를 학습했다. 이 쿼리를 기준으로 최대매출액 뿐만 아니라 최소 매출액과 해당사원을 조회하는 쿼리를 작성해보자.
```
SELECT emp.years  -- 연도
      ,emp.employee_id -- 사원 아이디
      ,emp2.emp_name -- 사원 이름
      ,emp.amount_sold  -- 매출액
FROM( 
    SELECT SUBSTR(a.sales_month, 1, 4) as years -- 연도 추출
    , a.employee_id
    , SUM(a.amount_sold) AS amount_sold
    FROM 
      sales a
    , customers b
    , countries c
    WHERE a.cust_id = b.cust_id
      AND b.country_id = c.country_id
      AND c.country_name = 'Italy'
    GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id
    ) emp,-- 
    (SELECT years,
             MAX(amount_sold) AS max_sold
     FROM (  -- 인라인 뷰
       SELECT  
              SUBSTR(a.sales_month, 1, 4) as years
            , a.employee_id
            , SUM(a.amount_sold) AS amount_sold
        FROM 
             sales a
           , customers b
           , countries c
        WHERE a.cust_id = b.cust_id
        AND b.country_id = c.country_id
        AND c.country_name = 'Italy'
      GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id
      ) K 
      GROUP BY years
      ) sale,emp2 
WHERE emp.years = sale.years
  AND emp.amount_sold = sale.max_sold
  AND emp.employee_id = emp2.employee_id
ORDER BY years; 
```
답 :
```
SELECT emp.years  -- 연도
      ,emp.employee_id -- 사원 아이디
      ,emp2.emp_name -- 사원 이름
      ,emp.amount_sold  -- 매출액
FROM( 
    SELECT SUBSTR(a.sales_month, 1, 4) as years -- 연도 추출
    , a.employee_id
    , SUM(a.amount_sold) AS amount_sold
    FROM 
      sales a
    , customers b
    , countries c
    WHERE a.cust_id = b.cust_id
      AND b.country_id = c.country_id
      AND c.country_name = 'Italy'
    GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id
    ) emp,-- 
    (SELECT years,
             MAX(amount_sold) AS max_sold
             MIN(amount_sold) AS min_sold
     FROM (  -- 인라인 뷰
       SELECT  
              SUBSTR(a.sales_month, 1, 4) as years
            , a.employee_id
            , SUM(a.amount_sold) AS amount_sold
        FROM 
             sales a
           , customers b
           , countries c
        WHERE a.cust_id = b.cust_id
        AND b.country_id = c.country_id
        AND c.country_name = 'Italy'
      GROUP BY SUBSTR(a.sales_month, 1, 4), a.employee_id
      ) K 
      GROUP BY years
      ) sale,emp2 
WHERE emp.years = sale.years
  AND (emp.amount_sold = sale.max_sold OR emp.amount_sold = sale.min_sold)
  AND emp.employee_id = emp2.employee_id
ORDER BY years; 
```
최소매출액을 조회하게 하려면 `MIN` 함수를 이용해주면 된다. `sale` 쿼리에서 매출의 총 합계인 `amount_sold`에 `MIN`을 적용해 준뒤 마지막 부분의 `조인 조건`에서 `emp` 서브 쿼리의 `amount_sold`와 연결 시켜주면 된다.


# 7장
## 1번
문제 :
계층형 쿼리 응용편에서 LISTAGG 함수를 사용해 다음과 같이 ROW를 COlUMN으로 분리했다.
```
SELECT department_id, LISTAGG(emp_name, ',') WITHIN GROUP (ORDER BY emp_name) as empnames
      FROM employees
     WHERE department_id IS NOT NULL
     GROUP BY department_id;
```
LISTAGG 함수 대신 계층형 쿼리, 분석 함수를 사용해서 위 쿼리와 동일한 결과를 산출하는 쿼리를 작성해 보자.

답:
```
SELECT department_id, 
       SUBSTR(SYS_CONNECT_BY_PATH(emp_name, ','),2) empnames -- 어떤 경로가 찍혀서, 2칸이 잘리는건지?
 FROM ( SELECT emp_name, 
               department_id, 
               COUNT(*) OVER ( partition BY department_id ) cnt, 
               ROW_NUMBER () OVER ( partition BY department_id order BY emp_name) rowseq 
          FROM employees
         WHERE department_id IS NOT NULL) 
 WHERE rowseq = cnt 
 START WITH rowseq = 1 
CONNECT BY PRIOR rowseq + 1 = rowseq 
    AND PRIOR department_id = department_id; 
```
## 2번
문제:
다음 쿼리는 사원 테이블에서 JOB_ID가 `SH_CLERK`인 사원을 조회하는 쿼리이다.
```
SELECT employee_id, emp_name, hire_date
FROM employees
WHERE job_id = 'SH_CLERK'
ORDER By hire_date;
```
사원 테이블에서 퇴사일자(retire_date)는 모두 비어 있는데, 위 결과에서 사원번호가 184번인 사원의 퇴사일자는 다음으로 입사일자가 빠른 192번 사원의 입사일자라고 가정해서 다음과 같은 형태로 결과를 추출하도록 쿼리를 작성해 보자(입사일자가 가장 최근인 183번 사원의 퇴사일자는 NULL이다).
답 :
```
SELECT employee_id, emp_name, hire_date,
LEAD(hire_date, 1, null) OVER (ORDER BY hire_date) AS Retire_DATE
FROM employees
WHERE job_id = 'SH_CLERK'
ORDER BY hire_date;
```
`LEAD`는 다른 ROW의 값을 참조할 때 사용하는 함수로,
뒤에 있는 ROW의 값을 참조할 때 사용한다.
`LEAD`를 이용하여 한 칸 뒤에 있는 입사 날짜를 참조하여 퇴사 날짜를 만들었다.
## 3번
질문 :
sales 테이블에는 판매 데이터, customers 테이블에는 고객정보가 있다. 2001년 12월(SALES_MONTH = ‘200112’) 판매 데이터 중 현재일자를 기준으로 고객의 나이(customers.cust_year_of_birth)를 계산해서 다음과 같이 연령대별 매출금액을 보여주는 쿼리를 작성해 보자.

답 :
```
WITH basis AS ( SELECT WIDTH_BUCKET(to_char(sysdate, 'yyyy') - b.cust_year_of_birth, 10, 90, 8) AS old_seg, s.amount_sold
   FROM sales s, 
        customers b
   WHERE s.sales_month = '200112'
   AND s.cust_id = b.CUST_ID
    ),
 real_data AS ( SELECT old_seg * 10 || '대' AS              old_segment,  SUM(amount_sold) as old_seg_amt
              FROM basis GROUP BY old_seg
              )
 SELECT * FROM real_data
 ORDER BY old_segment;   
```
`sysdate`를 `to_char`를 이용해 문자열 타입으로 전환한 다음,  `cust_year_of_birth`를 `sysdate`에서 빼주어서
현재 나이를 계산해준다음`WIDTH_BUCKET`으로 나이대를 나눈 것을 `old_seg`로 이름 붙인다.  2001년 12월의 판매 데이터로 조건을 걸어준다.

그 다음 `old_seg`와 `amount_sold`를 이용해서 매출금액을 표현해준다.

## 4번 
```
WITH basis AS ( SELECT c.country_region, s.sales_month, SUM(s.amount_solD) AS amt
                  FROM sales s, 
                       customers b,
                       countries c
                 WHERE s.cust_id = b.CUST_ID
                   AND b.COUNTRY_ID = c.COUNTRY_ID
                 GROUP BY c.country_region, s.sales_month
              ),
     real_data AS ( SELECT sales_month, 
                           country_region,
                           amt,
                           RANK() OVER ( PARTITION BY sales_month ORDER BY amt ) ranks
                      FROM basis
                   )
 select *
 from real_data
 where ranks = 1;
 ```

## 5번

```
WITH basis AS (
SELECT REGION, GUBUN,
       SUM(AMT1) AS AMT1, 
       SUM(AMT2) AS AMT2, 
       SUM(AMT3) AS AMT3, 
       SUM(AMT4) AS AMT4, 
       SUM(AMT5) AS AMT5, 
       SUM(AMT6) AS AMT6, 
       SUM(AMT6) AS AMT7 
  FROM ( 
         SELECT REGION,
                GUBUN,
                CASE WHEN PERIOD = '201111' THEN LOAN_JAN_AMT ELSE 0 END AMT1,
                CASE WHEN PERIOD = '201112' THEN LOAN_JAN_AMT ELSE 0 END AMT2,
                CASE WHEN PERIOD = '201210' THEN LOAN_JAN_AMT ELSE 0 END AMT3, 
                CASE WHEN PERIOD = '201211' THEN LOAN_JAN_AMT ELSE 0 END AMT4, 
                CASE WHEN PERIOD = '201212' THEN LOAN_JAN_AMT ELSE 0 END AMT5, 
                CASE WHEN PERIOD = '201310' THEN LOAN_JAN_AMT ELSE 0 END AMT6,
                CASE WHEN PERIOD = '201311' THEN LOAN_JAN_AMT ELSE 0 END AMT7
         FROM KOR_LOAN_STATUS
       )
GROUP BY REGION, GUBUN
)   
SELECT REGION, 
       GUBUN,
       AMT1 || '( ' || ROUND(RATIO_TO_REPORT(amt1) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201111",
       AMT2 || '( ' || ROUND(RATIO_TO_REPORT(amt2) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201112",
       AMT3 || '( ' || ROUND(RATIO_TO_REPORT(amt3) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201210",
       AMT4 || '( ' || ROUND(RATIO_TO_REPORT(amt4) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201211",
       AMT5 || '( ' || ROUND(RATIO_TO_REPORT(amt5) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201212",
       AMT6 || '( ' || ROUND(RATIO_TO_REPORT(amt6) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201310",
       AMT7 || '( ' || ROUND(RATIO_TO_REPORT(amt7) OVER ( PARTITION BY REGION ),2) * 100 || '% )' AS "201311"
FROM basis
ORDER BY REGION;
```


