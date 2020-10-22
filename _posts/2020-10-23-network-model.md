---
layout: post
title: "네트워크 모델"
author: "DAEUN"
---

![계층적 통신](/assets/images/communications.PNG)

각 계층을 지나며 헤더를 추가하는 과정을 **캡슐화**라고한다.

<br>

- 특징
    - 계층적 구조(모듈화 ⇒ 구현, 유지, 보수, 업데이트 쉬워짐)
    - 계층마다 기능(protocol) 존재하며, 이 기능은 독립적(다른 계층이 그 기능을 대신하거나 관여할 수 없음)
    - 수평적 방법(horizontal manner): 양 끝(end systems)은 짝을 이루며 서로 반대로 기능 수행
    - 상위 계층은 하위 계층의 서비스를 받음

<br>

## OSI 7계층

- OSI : Open Systems Interconnection
- 1980년대 초 컴퓨터 시스템 간 통신을 위한 표준으로 사용되었다. 현재는 보다 단순해진 TCP/IP 모델을 사용한다.

![https://upload.wikimedia.org/wikipedia/commons/2/21/Osi-model-7-layers.png](https://upload.wikimedia.org/wikipedia/commons/2/21/Osi-model-7-layers.png)

<br>

![https://www.imperva.com/learn/wp-content/uploads/sites/13/2020/02/OSI-vs.-TCPIP-models.jpg](https://www.imperva.com/learn/wp-content/uploads/sites/13/2020/02/OSI-vs.-TCPIP-models.jpg)

<br><br>

### 7. Application Layer

사용자에게 **서비스를 제공**한다. 사용자가 요청을 받아 적절한 데이터를 보여준다. 관련된 프로토콜은 **HTTP**(Hypertext Transfer Protocol), **FTP**(File Transfer Protocol), **POP**(Post Office Protocol), **SMTP**(Simple Mail Transfer Protocol), **DNS**(Domain Name System) 등이 있다.

<br>

### 6. Presentation Layer

사용자가 이해할 수 있는 적절한 데이터로 변환한다. 인코딩, 디코딩, 암호화가 이루어지며 JPEG, GIF 등의 포맷에 맞춘다.

<br>

### 5. Session Layer

기기간 통신을 하는 길(communication channel)인 세션을 생성, 유지, 종료, 중단 시 복구한다.

<br>

### 4. Transport Layer

**프로세스 간 세그먼트를 전달**한다. 세그먼트는 이전 계층(Application or Session)에서 전달받은 data를 더 작은 조각(chunk)로 나눈 후 송/수신지 **포트번호**를 포함한 헤더를 더해 만든 데이터 단위. 관련 프로토콜은 **TCP, UDP** 존재.

![segment](/assets/images/segment.PNG)

<br>

### 3. Network Layer

**source host에서 destination host으로 datagram을 전달**한다. 데이터그램은 발신/수신 단말 장치와의 경로 지정을 위한 충분한 정보를 갖고 있는 **패킷**을 말한다. **IP 주소 결정, 라우팅(routing protocol 존재 → 경로 선택), 패킷 전달**을 한다.

![datagram](/assets/images/datagram.PNG)

<br>

### 2. Data Link Layer

네트워크 상 물리적으로 연결된 **노드(hop) 간 frame**(0과 1로 이루어진 의미 있는 덩어리)**을 전달**한다. **MAC 주소**를 이용해 통신한다. 관련 프로토콜에는 **Ethernet, 802.11(wifi), PPP**가 있다.

![frame](/assets/images/frame.PNG)

<br>

### 1. Physical Layer

네트워크 상 물리적으로 연결된 **노드(hop) 간 bit를 전달**한다. 데이터링크 계층에서는 0과 1로 이루어진 의미 있는 덩어리를 전달했다면, 물리 계층에서는 비트 자체를 전달한다.

![bit](/assets/images/bit.PNG)

<br>

[주소 체계 정리]

4. 전송 계층 → **포트 번호**로 프로세스 간 통신 (라우터가 이해 불가)

3. 네트워크 계층 → **IP 주소**로 호스트 간 통신

2. 데이터링크 계층 → **MAC 주소**로 물리적 통신

<br><br>

References: [OSI 7 계층](https://www.imperva.com/learn/application-security/osi-model/)