---
layout: post
title: "Go-Back-N"
author: "DAEUN"
---

## Sliding-Window Protocol : Go-Back-N, Selective-Repeat

---

Stop-and-Wait처럼 하나의 데이터를 보낸 후 그에 대한 응답을 기다리지 않고, ACK를 받지 않아도 일정 개수의 데이터를 보내는 pipelined 기법을 이용한 프로토콜

- 전송하는 여러 데이터를 구분하기 위한 **순서 번호(sequence #)**를 세그먼트 헤더에 저장
    - Stop-and-Wait에서는 현재 보낸 데이터와 이전에 보낸 데이터를 구분하기 위해 0과 1을 교차로 사용하면 됐었던 것과 차이가 있음
    - sequence #를 저장하는 필드가 m 비트라면,
        - $0$ ~ $2^m-1$ 까지 표현 가능
        - 즉, ACK 메시지를 받지 않고 한 번에 전송할 수 있는 최대 패킷의 개수는 $2^m$ 개

![sliding-window](/assets/images/sliding-window.png)

base가 가리키는 곳부터 윈도우 크기만큼 전송할 수 있다. 타임아웃 발생 시 base부터 재전송. base + N 이상인 패킷은 윈도우 크기를 넘어가므로 전송할 수 없다. 앞에서부터 차례대로 ACK 메시지를 받으면 윈도우는 한 칸씩 오른쪽으로 이동한다.

<br>

## Go-Back-N(GBN)

---

- cumulative ACK
    - ACK 메시지를 받은 패킷보다 앞선 패킷들 모두 잘 받았다는 의미
- 윈도우의 첫 번째 패킷(base가 가리키는 패킷)을 전송할 때, 타이머 start
    - 윈도우를 슬라이딩할 때마다 타이머 restart
- 시간초과 ⇒ base부터 재전송
- receiver의 수신창 크기 = **1**

<br>

### sender

![go-back-n-sender](/assets/images/go-back-n-sender.png)

1. 초기화 또는 모든 상황이 종료된 상태임을 의미
2. 응용 계층에서 데이터를 받았을 때,
    - 보낼 데이터 번호(nextseqnum)이 윈도우가 범위 안에 존재하면
        - 세그먼트를 만들어 네트워크 계층으로 전송
        - 윈도우의 첫 번째 패킷을 전송했으면
            - 타이머 start
        - nextseqnum 오른쪽으로 한 칸 이동
    - 현재 윈도우 크기만큼 패킷을 모두 전송했으면 데이터를 처리하지 못함
3. 패킷 손실 또는 데이터가 망가지면 **시간초과** 발생 ⇒ **재전송**
    - 타이머 start
    - 전송 못한 패킷(base)부터 그 뒤의 패킷(nextseqnum-1)까지 재전송
        - 수신 측에서 앞의 패킷이 망가지면 뒤의 패킷은 버리고 성공한 패킷까지의 ACK를 보내기 때문에 성공적으로 보낸 패킷을 다시 전송해도 문제 없음
        - Go-Back-N인 이유. 문제 발생 시 첫 번째 패킷으로 돌아간다.
4. 망가지지 않은 제어 메시지를 받았을 때,
    - base는 **(ACK 받은 패킷 번호) + 1**을 가리킨다. (누적된 ACK, 윈도우 슬라이딩)
    - 보낸 데이터에 대한 ACK 메시지를 모두 받았으면 ⇒ 타이머 종료 else 타이머 재시작
5. 망가진 제어 메시지를 받았을 때,
    - 아무 일도 하지 않고 시간초과되기를 기다림

<br>

### receiver

![go-back-n-receiver](/assets/images/go-back-n-receiver.png)

1. 초기화
2. 기대한 번호의 패킷이 아니라면 ⇒ 이전에 만들었던 ACK 메시지 전송(해당 번호-1의 패킷까지 잘 받았다는 의미) 즉, 기대했던 패킷이 아니라면 버림
3. 기대했던 번호의 패킷을 정상적으로 받았으면 ⇒ 응용 계층으로 데이터 전송 + 해당 패킷에 대한 ACK 메시지를 sender에게 전송 + 다음 번호의 패킷을 기다림

<br>

### GBN 동작

[.](https://lh3.googleusercontent.com/proxy/B1KROZtqkO_w4WVq1-S2outDGyI2Md4bYG-o4oMrpNQmC-nDCc7Mv0C1EemCW5C3-kcuU38asfiSj_K4DHt4KAw_hex6Nk-s4e7LZ5KiBPK-powrGCjU4-ZU-DzE6oQGYee9Y2rpf73uOwCPSRAIvm-2o931-G1sR_nxHCmpIFPs4YotJ6ZfX37uUfYb8cZI25u1upA)

- pkt2가 손실되었고, 뒤 이어 전송된 pkt3가 receiver에게 전송됨 ⇒ receiver는 pkt2를 기다리고 있었기에 이를 버리고 전에 만들었던 pkt1에 대한 ACK 메시지 전송
- ACK0, ACK1을 받으면 윈도우 한 칸씩 이동
- 타임아웃되면 pkt2부터 재전송

<br>

### 윈도우 크기가 $2^m$보다 작아야 하는 이유

[.](https://t1.daumcdn.net/cfile/tistory/1642E84750A1DE6A04)

- 윈도우 크기 < $2^m$ 일 때,
- receiver는 기대했던 패킷을 받고
    - 수신창 한 칸 이동 + ACK 메시지 전송
    - 그러나 ACK 손실
- pkt0에 대한 ACK 메시지를 받지 못한 sender는 기다리다 타임아웃
    - pkt0 재전송 ⇒ receiver가 기대한 패킷 번호가 아니므로 이전에 만든 ACK 메시지 보냄
- ⇒ **정상**적인 작동

- 윈도우 크기 = $2^m$ 일 때,
- receiver는 기대했던 패킷을 받고
    - 수신창 한 칸 이동 + ACK 메시지 전송
    - 그러나 ACK 손실
- pkt0에 대한 ACK 메시지를 받지 못한 sender는 기다리다 타임아웃
    - pkt0 재전송 ⇒ receiver가 기대한 패킷 번호와 같아 데이터 처리한다. 하지만 **패킷 번호만 같을 뿐, 다른 데이터이기 때문에 비정상적인 처리**