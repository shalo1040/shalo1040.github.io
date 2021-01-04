---
layout: post
title: "Selective-Repeat"
author: "DAEUN"
---

### sender

![.](https://www.myreadingroom.co.in/images/stories/docs/dcn/Selective%20Repeat%20Automatic%20Repeat%20Request_send%20window.JPG)

- 송신자의 윈도우 크기는 $2^{m-1}$
- ACK 메시지를 받지 않은 패킷만 재전송
    - 패킷마다 타이머 존재
    - GBN은 base 패킷부터 전체 재전송

<br>

### receiver

![.](https://www.myreadingroom.co.in/images/stories/docs/dcn/Selective%20Repeat%20Automatic%20Repeat%20Request_receive%20window.JPG)

- 수신자의 윈도우(수신버퍼) 크기는 $2^{m-1}$
- 수신한 패킷에 대한 ACK 전송
    - GBN은 누적된 ACK
- 패킷의 순서를 맞춘 후 응용 계층으로 전송

<br>

### SR 동작

![.](https://www.net.t-labs.tu-berlin.de/teaching/computer_networking/03.04-Dateien/sr_example1.gif)

- pkt2가 손실되었지만, receiver는 뒤이어 받은 pkt3,4를 버리지 않고 갖고 있는다.
- pkt2의 ACK를 받지 못해 타임아웃이 되어 pkt2 재전송

<br>

### 윈도우 크기가 최대 $2^{m-1}$이어야 하는 이유

[.](https://t1.daumcdn.net/cfile/tistory/13109D4F50A1F3CA0D)

- 윈도우 크기 = $2^{m-1}$ 일 때,
- receiver는 기대했던 패킷을 받고
    - 수신창 한 칸 이동 + ACK 메시지 전송
    - 그러나 ACK 손실
- pkt0에 대한 ACK 메시지를 받지 못한 sender는 기다리다 타임아웃
    - pkt0 재전송 ⇒ receiver가 기대한 패킷 번호가 아니므로 패킷 버림
- ⇒ **정상**적인 작동

- 윈도우 크기 > $2^{m-1}$ 일 때,
- receiver는 기대했던 패킷을 받고
    - 수신창 한 칸 이동 + ACK 메시지 전송
    - 그러나 ACK 손실
- pkt0에 대한 ACK 메시지를 받지 못한 sender는 기다리다 타임아웃
    - pkt0 재전송 ⇒ receiver가 기대한 패킷 번호와 같아 데이터 처리한다. 하지만 **패킷 번호만 같을 뿐, 다른 데이터이기 때문에 비정상적인 처리**