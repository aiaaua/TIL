# Pagination

`pagination`이란 한정된 네트워크 자원을 효율적으로 활용하기 위해 특정한 정렬 기준에 따라 데이터를 분할하여 가져오는 것을 의미함.  

`pagination`은 아래와 같은 두 가지 방식으로 처리 가능  

- Offset-based Pagination
- Cursor-based Pagination



## Offset-based Pagination

오프셋 기반의 페이지네이션은 DB에서 지정된 offset만큼을 페이지 단위로 구분하여 요청 및 응답을 하는 방식임.  

대부분의 DB에서 오프셋 기반의 페이지네이션을 제공하며,  

`OFFSET Query` 와 `LIMIT Query`에 콤마를 붙여 페이지네이션을 구현할 수 있음.  

```mysql
SELECT *
FROM {table}
WHERE {condition}
LIMIT {contents number} OFFSET {page number}
```

오프셋 기반 페이지네이션은 쉽고 편리하지만, 다음과 같은 문제점이 있음.  

- 페이지 요청 사이 데이터 변화가 있는 경우 중복 데이터 발생
- 대부분의 RDBMS에서 OFFSET 쿼리의 퍼포먼스 이슈



## Cursor-based Pagination

`Cursor`개념을 사용하여 사용자에게 응답해준 마지막 데이터 기준으로 다음 n개를 요청 및 응답을 하는 방식임.  

```mysql
SELECT *
FROM {table}
WHERE id < last_id AND {condition}
ORDER BY id DESC
FETCH FIRST {contents number} ROWS ONLY
```

커서 기반의 페이지네이션은 오프셋 기반보다 성능이 우수하며 오프셋 기반 페이지네이션의 문제점들을 해결할 수 있음.  

하지만 커서 기반의 페이지네이션은 임의의 특정 페이지로 바로 이동이 불가능하다는 문제점이 있음.  



## Reference

- https://wonyong-jang.github.io/database/2020/09/06/DB-Pagination.html
- https://binux.tistory.com/148
