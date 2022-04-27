﻿---
title : "Oracle의 다양한 연산자"
excerpt: "Oracle의 다양한 연산자에 대해 알아본다."
categories:
- Oracle
tags:
- [Oracle]
- [Operator]
date: 2022-04-27 10:00:00
---
# 연산자
연산자는 연산을 수행하는 역할이다.
## 수식 연산자
`+` `-` `*` `/` 의 수식 연산자가 존재한다.
## 문자 연산자
`||`는 두 문자를 붙이는 역할이다.
```
SELECT employee_id || '-' || emp_name AS employee_info
 FROM employees
WHERE ROWNUM < 5;
```
## 논리 연산자
`>`  `<` `>=` `<=` `=`의 부등호가 존재하며,
`<>` `!=` `^=` 의 부등호는 모두 "같지 않다" 라는 의미로 쓰인다 .
## 집합 연산자
집합 연산자는 나중에~
## 계층형 쿼리 연산자
계층형 쿼리 연산자는 나중에~
