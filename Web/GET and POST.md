# GET and POST

GET과 POST는 모두 브라우저가 서버에 요청하는  HTTP Method임  

## GET Method

서버로부터 정보를 조회하기 위해 설계된 method  

요청을 전송할 때 필요한 데이터를 쿼리스트링을 통해 전달(따라서 요청 데이터 길이에 대한 제한이 있음)  

불필요한 요청을 제한하기 위해 결과를 캐시할 수 있음(주로 정적 컨텐츠를 캐시해두고 동일한 요청이 발생할 경우 서버로 요청을 보내지 않고 캐시 데이터 사용)  

요청을 북마크에 추가할 수 있으며 브라우저에 기록이 남음  

요청이 성공할 경우 HTTP status code로 200을 반환하며 json, xml등 여러 형식의 데이터를 반환  

`Idempotent` 하므로 서버에 동일한 요청을 여러 번 수행하더라도 동일한 결과가 나타남  



## POST Method

리소스를 생성 및 변경하기 위해 설계된 method  

요청을 전송할 때 필요한 데이터를 HTTP의 Body에 담아서 전달(따라서 길이 제한 없이 데이터 전송 가능)  

요청 헤더의 Content-Type에 요청 데이터의 타입을 표시해야함  

결과를 캐시할 수 없고, 요청을 북마크에 추가할 수 없으며, 브라우저에 기록도 남지 않음  

요청에 따른 자원이 생성될 경우 HTTP status code로 201을 반환함  

`Non-Idempotent` 하므로 서버에 동일한 요청을 여러 번 수행할 경우 응답이 항상 다를 수 있음  



## Difference

두 메서드의 차이는 아래와 같이 극명하므로 설계 원칙에 따라 적절하게 사용해야함  

|                           | GET         | POST                |
| ------------------------- | ----------- | ------------------- |
| **사용 시기**             | 리소스 요청 | 리소스 생성 및 변경 |
| **요청 데이터 전달 방식** | 쿼리스트링  | HTTP Body           |
| **요청 데이터 길이 제한** | O           | X                   |
| **캐시**                  | O           | X                   |
| **북마크**                | O           | X                   |
| **브라우저 기록**         | O           | X                   |
| **HTTP status code**      | 200(Ok)     | 201(Created)        |
| **Idempotent**            | O           | X                   |



## Reference

- https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/
- https://velog.io/@songyouhyun/Get%EA%B3%BC-Post%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%84%EC%8B%9C%EB%82%98%EC%9A%94

