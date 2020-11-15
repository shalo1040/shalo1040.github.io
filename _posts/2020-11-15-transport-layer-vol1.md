---
layot : post
title: "Multiplexing and Demultiplexing"
author: "DAEUN"
---

### 전송 계층의 프로토콜

- **TCP**
	- Transmission Control Protocol
	- **신뢰성** 있는 통신
	    - by 흐름제어, 혼잡제어
	- connection-oriented(연결 지향적, handshaking)

<br>

- **UDP**
	- User Datagram Protocol
	- 신뢰성 있는 통신 보장하지 않음
	- connectionless-oriented(비연결 지향적, no handshaking)

<br>

## 전송계층의 대표적 기능

---

[multiplexing and demultiplexing](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdOfjPm%2FbtqyRMuV7YY%2FKuybkmkKYmwsVQ9WeROXU1%2Fimg.png)

- **multiplexing** : 여러 소켓을 하나의 연결된 길로 지나갈 수 있도록 합침
- **demultiplexing** : 하나의 길로 들어온 소켓들을 알맞은 프로세스로 찾아갈 수 있도록 길을 나눠줌

<br>

## Connectionless Transport(UDP)

![udp](/assets/images/udp.png)

<br>

- 화살표 방향은 **단방향**
    - 연결을 유지하지 않고 클라이언트가 요청한 정보에 서버가 응답할 뿐임 (connectionless)
- 필요한 정보는 **2개**
    - source port 주소
    - destination port 주소

<br>

## Connection Transport(TCP)

![tcp](/assets/images/tcp1.png)

<br>

- 화살표 방향은 **양방향**
    - 연결을 유지한 채로 통신 (connection)

- 필요한 정보는 **4개**
    - source IP 주소
    - source port 주소
	- destination IP 주소
	- destination port 주소

<br>

- 위 그림에서 A와 C가 B에 통신을 하면서 서로 다른 소켓으로 demultiplex
    - 하지만 웹 서버와 같이 많은 사람들이 동시에 접근하는 경우에는 비효율적이므로 다음과 같이 **스레드**를 이용한다.

![tcp](/assets/images/tcp2.png)