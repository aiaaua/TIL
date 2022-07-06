## DocumentQuery_find()

#### find()

db.[COLLECTION_NAME].find([QUERY, PROJECTION])

> QUERY : optional, 조회의 조건을 작성
>
> PROJECTION : optional, 조회 시 보여질 field 설정



#### QUERY 연산자

1. 비교(Comparison) 연산자

| operator | full                   | description                     |
| -------- | ---------------------- | ------------------------------- |
| $eq      | equals                 | 주어진 값과 일치하는 값         |
| $ne      | not equal              | 주어진 값과 일치하지 않는 값    |
| $gt      | greater than           | 주어진 값보다 큰 값             |
| $gte     | greater than or equals | 주어진 값보다 크거나 같은 값    |
| $lt      | less than              | 주어진 값보다 작은 값           |
| $lte     | less than or equals    | 주어진 값보다 작거나 같은 값    |
| $in      | in                     | 주어진 배열 안에 속하는 값      |
| $nin     | not in                 | 주어진 배열 안에 속하지 않는 값 |

2. 논리(Logical) 연산자

| operator | description                           |
| -------- | ------------------------------------- |
| $or      | 주어진 조건 중 하나라도 true이면 true |
| $and     | 주어진 모든 조건이 true이면 true      |
| $not     | 주어진 조건이 false이면 true          |
| $nor     | 주어진 모든 조건이 false이면 true     |

3. 평가(Evaluation) 연산자

| operator | description                       |
| -------- | --------------------------------- |
| $regex   | 지정한 정규식과 일치하는 문서     |
| $where   | javascript 표현식과 일치하는 문서 |

4. 배열(Array) 연산자

| operator   | description                              |
| ---------- | ---------------------------------------- |
| $elemMatch | embedded documents 배열을 쿼리할 때 사용 |


---
#### [reference](https://velopert.com/479)

