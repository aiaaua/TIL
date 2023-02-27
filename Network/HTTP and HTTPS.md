# HTTP and HTTPS

## HTTP

`HTTP(HyperText Transfer Protocol)` 는 서버/클라이언트 모델을 따라 데이터를 주고받기 위한 프로토콜로 80번 포트를 사용  

애플리케이션 레벨의 프로토콜(TCP/IP 위에서 작동함)  

상태를 가지고 있지 않은 stateless 프로토콜  

암호화되지 않은 평문 데이터를 전송하는 프로토콜  

`Method`, `Path`, `Version of the protocol`, `Header`, `Body` 등으로 구성  



## HTTPS

`HTTPS(HyperText Transfer Protocol Secure)` 은 `HTTP` 에 데이터 암호화가 추가된 프로토콜로 443번 포트를 사용  

암호화된 데이터를 전송하는 프로토콜



## Diff

두 프로토콜 사이에 가장 커다란 차이점은 **SSL인증서**  

SSL 인증서는 사용자가 사이트에 제공하는 정보를 암호화하므로 중간에 데이터가 탈취되어도 암호화되어있어 해독할 수 없음  

또한 `HTTPS`는 TLS 프로토콜을 통해서도 보안을 유지  

TLS 프로토콜은 데이터 무결성을 보장하기 때문에 데이터가 전송 중 수정되거나 손상되는 것을 방지하며, 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증할 수 있음  

따라서 신용카드 정보나 비밀번호 등 사용자들의 민감한 정보들을 다루는 웹사이트라면 `HTTPS`를 사용하는 것이 좋음  



## Reference

- https://mangkyu.tistory.com/98
- https://brunch.co.kr/@hyoi0303/10
- 