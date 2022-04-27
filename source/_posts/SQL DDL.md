---
title: "SQL DML 알아보기"
excerpt: "Oracle SQL의 대표적인 데이터 조작 언어를 알아본다."
categories:
- Oracle
tags:
- [Oracle]
- [DML]
date: 2022-04-26 13:00:00
---
# 대표적인 DML
## SELECT
테이블이나 뷰에 있는 데이터를 조회할 때 `SELECT`를 사용한다.
```
SELECT * 혹은 column 
 FROM [스키마.]테이블명 혹은 [스키마.]뷰명
WHERE 조건
ORDER BY column;
```
다수의 테이블에서 데이터 조회하기
```
SELECT a.employee_id, a.emp_name, a.department_id,
           b.department_name
      FROM employees a,
           departments b
     WHERE a.department_id = b.department_id;
```
`a`와 `b`는 `employees`와 `departments` 대신 사용되는 별칭이다. column 명에 별칭을 붙일때에는 `기존 column명 AS column별칭`의 형태로 사용한다.

## INSERT 
데이터의 신규 입력의 경우 `INSERT`를 사용하는데,
크게 기본 형태, column명 생략 형태, INSERT~SELECT 형태로 나눌 수 있다.

### 기본 형태
```
INSERT INTO [스키마.]테이블명 (column1,column2...)
VALUES (값1, 값2..);
```
### column명 생략 형태
```
INSERT INTO [스키마.]테이블명
VALUES (값1, 값2, ...);
```
### INSERT~SELECT 형태
```
INSERT INTO [스키마.]테이블명 (column1,column2...)
SELECT 문;
```
## UPDATE
테이블의 기존 데이터를 수정할 때 `UPDATE`를 사용한다
```
UPDATE [스키마.]테이블명
SET 컬럼1 = 변경값1,
    컬럼2 = 변경값2,
...
WHERE 조건;
```
## MERGE 
조건을 비교해서 테이블에 해당 조건에 맞는 데이터가 없으면 `INSERT` 있으면 `UPDATE`를 수행한다.
```
MERGE INTO [스키마.]테이블명
  USING (update나 insert될 데이터 원천)
    ON (update될 조건)
WHEN MATCHED THEN
   SET column1 = 값1, column2 = 값2, ...
WHERE update 조건
   DELETE WHERE update_delete 조건
WHEN NOT MATCHED THEN
  INSERT (column1,column2...) VALUES (값1, 값2,...)
  WHERE insert 조건;
```

## DELETE
테이블에 있는 데이터를 삭제할 때 `DELETE`를 사용한다.
```
-- 일반적으로 쓰임
DELETE [FROM] [스키마.]테이블명
WHERE delete 조건; 

-- 특정 파티션만 삭제 할 경우
DELETE [FROM] [스키마.]테이블명 PARTITION (파티션명)
WHERE delete 조건;
```
## COMMIT & ROLLBACK 
`COMMIT`는 변경한 데이터를 데이터베이스에 마지막으로 반영할 때
`ROLLBACK`는 반대로 변경한 데이터를 변경 전으로 되돌릴 때 사용한다.
```
COMMIT [WORK] [TO SAVEPOINT 세이브포인트명] ;
ROLLBACK [WORK] [TO SAVEPOINT 세이브포인트명] ;
```

## TRUNCATE 
일반적으로 사용되는 `DELETE`문의 경우 `COMMIT`를 실행해야 데이터가 완전히 삭제되고, `ROLLBACK`을 실행하면 데이터가 다시 복구된다.
그러나 `TRUNCATE`문의 경우 데이터가 바로 삭제되고, `ROLLBACK`을 실행해도 데이터가 복구되지 않는다. 또한 `WHERE`조건문을 쓸 수 없으므로 항상 테이블 데이터 전체가 삭제된다.
```
TRUNCATE TABLE [스키마명.]테이블명;
```

