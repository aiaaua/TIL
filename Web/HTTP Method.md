# HTTP Method
HTTP Method란 클라이언트와 서버 사이에 이루어지는 요청(Request)와 응답(Response) 데이터를 전송하는 방식을 말함  

## 주요 Method
### GET
리소스 조회  

### POST
요청 데이터 처리, 주로 등록에 사용  

### PUT
리소스 전체 변경(덮어쓰기), 해당 리소스가 없으면 생성  

### PATCH
리소스 부분 변경  

### DELETE
리소스 삭제  


## 보조 Method
### HEAD
GET과 동일하나 리소스 조회 시 body를 제외하고 상태줄과 헤더만 조회  

### OPTIONS
대상 리소스에 대한 통신가능 옵션 설정  

### CONNECT
대상 자원으로 식별되는 서버에 대한 터널을 설정  

### TRACE
대상 리소스에 대한 경로를 따라 메세지 루프백 테스트 실행  


## HTTP Method 취약점
