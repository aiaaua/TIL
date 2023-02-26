# 3-Way Handshake and 4-Way Handshake

3-Way Handshake 는 TCP의 접속,4-Way Handshake는 TCP의 접속 해제 과정임  

## 3-Way Handshake

TCP 통신을 이용하여 데이터를 전송하기 전에, 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정  

TCP 통신은 PAR(Positive Acknowledgement with Retransmission)을 사용하는데,  

PAR은 Postive ACK를 받을 때까지 데이터 유닛을 재전송하기 때문에 수신자가 받은 데이터 유닛이 손상되어있다면 Positive ACK를 보내지 않으므로 보다 신뢰적인 통신을 제공함  

### Work

![image](https://user-images.githubusercontent.com/55227276/220655923-75af247a-ae56-4e3a-9397-9f425e8566da.png)

#### 1. SYN

클라이언트가 서버와 커넥션하기 위해   

임의의 랜덤 숫자인 `Sequence Number`를 지정하고, 플래그 비트를 1로 설정한 세그먼트를 서버로 전송  

(여기서 `Sequence Number`인 `ISN`을 난수를 설정해서 설정하는 이유는 connection을 맺을 때 사용하는 port가 유한 범위 내에 있어 시간이 지남에 따라 재사용되는데,   

이 과정에서 두 통신 호스트가 과거에 사용된 포트 번호 쌍을 사용할 가능성이 있음.  

그래서 서버는 SYN플래그를 보고 패킷을 구분하게 되는데, 난수가 아닌 순차적인 숫자를 전송한다면 이전의 connection으로부터 오는 패킷으로 잘못 인시할 수 있기 때문.)  

- Port 상태
  - 클라이언트: `CLOSED -> SYN_SENT`
  - 서버: `LISTEN`

#### 2. SYN + ACK

서버가 SYN을 받고, 이를 받았다는 신호 및  포트를 열어달라는 메세지로   

`ACK Number`를 `Sequence Number + 1`으로 지정하고, SYN과 ACK의 플래그비트를 1로 설정한 세그먼트를 클라이언트에게 전송

- Port 상태
  - 클라이언트: `SYN_SENT -> ESTABLISHED`
  - 서버: `SYN_RECEIVED`

#### 3. ACK

서버가 ACK를 받고 연결을 맺는 단계

- Port 상태
  - 클라이언트: `ESTABLISHED`
  - 서버: `SYN_RECEIVED -> ESTABLISHED`



## 4-Way Handshake

클라이언트와 서버의 연결을 해제(Connection Termination)하는 과정  

TCP는 `Abrupt Connection Release`와 `Graceful Connection Release` 두 가지 연결 해제 방식이 있는데, 이 중에 `Graceful Connection Release`일 때 4-Way Handshake가 발생함  

### Work

![image](https://user-images.githubusercontent.com/55227276/220962208-18c7aee9-1270-4cc0-af14-57c4add9638b.png)

#### 1. FIN + ACK

클라이언트가 `close()`를 호출하여 접속을 끊고 서버에게 FIN플래그를 보냄  

클라이언트는 FIN플래그를 보낸 뒤 `FIN_WAIT_1` 상태가 됨(서버의 ACK 플래그를 기다리는 상태)  

#### 2. ACK

서버가 FIN 플래그를 받고 클라이언트에게 ACK 플래그를 보냄  

(`ACK Number` 필드를 `Sequence Number + 1` 로 지정하고 ACK플래그 비트를 1로 설정한 세그먼트를 전송)  

서버는 이 응답을 보낸 뒤 `CLOSE_WAIT` 상태가 되며 전송할 남은 데이터가 있다면 마저 전송 후 `close()`를 호출  

클라이언트는 서버의 ACK플래그를 받은 뒤 `FIN_WAIT_2` 상태가 됨(서버의 FIN 플래그를 기다리는 상태)  

#### 3. FIN

서버에서 남은 데이터를 모두 보낸 뒤 연결 종료에 합의한다는 의미로 FIN 플래그를 보냄  

서버는 이 응답을 보낸 뒤 `LAST_ACK` 상태가 됨(클라이언트의 ACK 플래그를 기다리는 상태)  

#### 4. ACK

클라이언트가 FIN 플래그를 받고 서버에게 ACK 플래그를 보냄  

서버는 클라이언트의 ACK 플래그를 받은 뒤 `CLOSED` 상태로 들어감  

혹시 서버로부터 아직 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT` 상태로 기다리며, 이는 의도치 않은 에러로 인해 연결이 데드락에 빠지는 것을 방지하기 위한 상태임  

시간이 초과되면(default: 240sec) 클라이언트는  `CLOSED` 상태로 들어감  



## Reference

- https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
- https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake

- https://seongonion.tistory.com/74D
