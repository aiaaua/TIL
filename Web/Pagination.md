# Pagination

`pagination`이란 한정된 네트워크 자원을 효율적으로 활용하기 위해 특정한 정렬 기준에 따라 데이터를 분할하여 가져오는 것을 의미함.  

`pagination`은 아래와 같은 두 가지 방식으로 처리 가능  

- Offset-based Pagination
- Cursor-based Pagination



## Offset-based Pagination

오프셋 기반의 페이지네이션은 DB에서 지정된 offset만큼을 페이지 단위로 구분하여 요청 및 응답을 하는 방식임.  

대부분의 DB에서 오프셋 기반의 페이지네이션을 제공하며,  

`OFFSET Query` 와 `LIMIT Query`에 콤마를 붙여 페이지네이션을 구현할 수 있음.  





## Cursor-based Pagination



## Reference

- https://wonyong-jang.github.io/database/2020/09/06/DB-Pagination.html
- https://binux.tistory.com/148