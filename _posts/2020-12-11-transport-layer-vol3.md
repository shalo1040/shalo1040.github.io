---
layout : post
title : "TCP"
author : "DAEUN"
---

- **연결지향** 프로토콜
    - 3-way handshaking
- **양방향** 통신(full-duplex)
    - 예) Telnet
- **pipelining** 기법
- **신뢰성** 있는 통신
    - Error Detection
        - 헤더의 checksum을 이용해 헤더와 데이터의 오류 검출
    - Retransmissions
        - 패킷 손실 또는 오류 검출되면 재전송
    - Cumulative Acknowledgements
        - Go-Back-N의 **누적 ACK**를 사용. 앞 번호의 패킷은 모두 잘 받았음을 의미
    - Timers
        - 타이머로 오류 감지
        - **Single-Timer**
    - Header Fields(sequence #, ACK #)

<br>

```
TCP 프로토콜은 GBN과 SR 중 하나만 사용하지 않고 둘 다 섞어서 쓴다.

GBN -> 누적된 ACK, Single-Timer
SR -> 손실된 패킷만 재전송
	n번째 패킷의 타임아웃 전에 n+1 패킷의 ACK가 도착하면, n번째 패킷 재전송하지 않음
```

<br>

### sender

1) 응용 계층으로부터 데이터 받았을 때,
	- 데이터의 첫 바이트를 seq #으로 설정
	- if(base==nextseqnum) start timer;
	- 타이머는 가장 먼저 전송한 패킷 기준
	- 타이머 시간은 운영체제가 결정

2) 타임아웃 됐을 때,
	- 타임아웃 된 패킷 재전송
	- 타이머 restart
	- 혼잡 인지/제어를 위한 타이머 활용
		- doubling the timeout interval
		- fast retransmit

3) ACK 메시지 받았을 때,
	- ACK 받은 패킷보다 앞서 전송한 패킷까지 모두 잘 받았다고 인식
	- sliding-window
	- 타이머 restart


<br>

### receiver


1) 기대한 seq # **순서대로** 패킷 받을 때,
	- delayed ACK up to 500ms
		- 잠시 ACK를 보내지 않고 **기다린** 후,
		- 다음 패킷이 도착하면,
			- 마지막 패킷에 대한 ACK 메시지
			- => 네트워크 혼잡도 줄임(**누적된 ACK**)

2) 1번의 대응 상황처럼 pending 된 ACK 있을 때,
	- 누적된 ACK 전송

3) 기대한 패킷보다 나중의 것을 받았을 때,
	(Gap Detected)
	- duplicate ACK 전송 (GBN 방식)
	- sender는 손실된 패킷만 전송 (SR 방식)

4) 3번의 gap이 채워졌을 때,
	- 누적된 ACK 전송
	- 수신창의 윈도우 슬라이딩


<br>

## TCP 헤더 구조

![https://www.dcs.bbk.ac.uk/~ptw/teaching/IWT/transport-layer/tcp-segment-structure.gif](https://www.dcs.bbk.ac.uk/~ptw/teaching/IWT/transport-layer/tcp-segment-structure.gif)

- **sequence #** : 순서 번호
    - 세그먼트의 첫 번째 바이트를 sequence #로 지정한다. (바이트 지향) 즉, 번호 순서대로 0,1,2,3,...이 아닌, 0,1401, 2801,... 형식
    - 32비트 차지 ⇒ 범위 : $0$ ~ $2^{32}-1$
- **acknowledgement #** : ACK 응답 번호
    - sender 입장
        - (acknowledgement # - 1)까지 잘 전달되었다고 인식 (누적된 ACK)
    - receiver 입장
        - (sequence # + 1)을 받기를 기대한다고 전달
    - flag의 **ACK**가 활성화 되었을 때 유효

<br>

```
[sequence #와 acknowledgement #가 헤더에 동시에 존재하는 이유]
	TCP는 **양방향** 통신을 하기 때문.
	즉, 수신측이 송신측, 송신측이 수신측이 될 수 있다.
	sender는 sequence #를, receiver는 acknowledgement #를 전송

	송수신을 동시에 하는 것을 데이터에 데이터를 얹어 보낸다는 의미로 **piggybacking**이라고 함.
```

<br>

- header length : TCP 헤더의 크기. 하지만 **20 바이트**로 거의 고정되어 사용하지 않음
- receive window : **수신 윈도우의 버퍼 크기** 지정(바이트 단위)
- checksum : **헤더**와 **데이터**의 오류 검출
- urgent pointer : 긴급 데이터 처리. flag 중 **URG**가 활성화 되었을 때 유효
- flag
    - URG : urgent pointer 필드에 값이 있음을 알림
    - **ACK** : acknowledgement # 필드에 값이 있음을 알림
    - PSH : push. 현재 세그먼트의 데이터를 즉시 상위 계층에 전달하도록 지시
    - RST : reset. 연결 재설정 필요 의미
    - **SYN** : 연결 설정 요청(3-way handshaking)
    - **FIN** : finish. 연결 종료 요청(3-way handshaking)

<br><br>

references: [TCP Header](http://www.ktword.co.kr/abbr_view.php?m_temp1=1889)