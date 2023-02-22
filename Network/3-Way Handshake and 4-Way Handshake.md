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





## Reference

- https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake
- https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake

- https://seongonion.tistory.com/74
