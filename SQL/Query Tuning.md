# Query Tuning

## Case 1

인덱스 컬럼을 조건절에서 좌변 가공하는 경우 인덱스를 타지 않고 Table Full Scan을 하므로 이를 지양해야 함  

```sql
SELECT * 
FROM 업체
WHERE substr(업체명, 1, 2) = '대한'
```

## Case 2

부정형 비교를 하는 경우 인덱스를 타지 않고 Table Full Scan을 하므로 이를 지양해야 함  

```sql
SELECT * 
FROM 고객
WHERE 직업 <> '학생'
```

## Case 3

`IS NOT NULL` 조건 사용하는 경우 인덱스를 타지 않고 Table Full Scan을 하므로 이를 지양해야 함  

```sql
SELECT * 
FROM 사원
WHERE 부서코드 IS NOT NULL
```

## Case 4

한 테이블에 대해 두 개의 인덱스를 가지고 있을 때 EQUAL 비교와 범위 비교를 동시에 할 경우 Oracle은 인덱스 들에 대해 merge를 하지 않으므로 다른 인덱스를 사용해야 함  

```sql
SELECT NAME
FROM EMP
WHERE DEPT_NO > 20
	AND EMP_CAT = 'A'
```

> 아래 조건인 `EMP_CAT`에 대한 인덱스만 사용



## Reference

- https://youngram2.tistory.com/70
- https://hyunipad.tistory.com/m/100