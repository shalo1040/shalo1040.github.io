---
layout : post
title: "신뢰성 있는 통신 : RDT"
author: "DAEUN"
---


### 데이터 전송 방식

![pipelined](/assets/images/pipelined.png)

데이터를 전송할 수 있는 방법은 stop-and-wait와 pipelining이 있다. 전자는 하나의 데이터를 전송하고 그에 대한 제어 메시지를 받을 때까지 대기하는 방법, 후자는 한 번에 여러 데이터를 전송하는 방법이다. stop-and-wait는 **RDT**, pipelined는 sliding-window protocol(**Go-Back-N**, **Selective-Repeat**)가 있다.

<br><br>

# 신뢰성 있는 통신 : Reliable Data Transfer(RDT)

---

- sender
    - application layer에서 transport layer로 데이터를 전송할 때 : **rdt_send()**
    - transport layer에서 network layer로 데이터를 전송할 때 : **udt_send()**
- receiver
    - network layer에서 transport layer로 데이터를 전송할 때 : **rdt_rcv()**
    - transport layer에서 application laer로 데이터를 전송할 때 : **deliver_data()**

<br>

- **rdt**는 **단방향** 전송
- **제어 메시지**는 **양방향** 전송

<br><br>

## RDT 1.0 : errorless 환경

![rdt1](/assets/images/rdt1.png)

<br>

- sender
    - 응용 계층에서 오는 데이터를 기다리고 있다.
    - rdt_send()로 데이터가 들어오면 적절한 헤더를 붙여 세그먼트를 만들고 네트워크 계층으로 udt_send()한다.
- receiver
    - 네트워크 계층에서 오는 데이터를 기다리고 있다.
    - rdt_rcv()로 데이터가 들어오면 헤더와 payload를 분리해 응용 계층으로 deliver_data()한다.


<br>

## RDT 2.0 : Stop-and-Wait

- RDT 1.0과 달리, 데이터의 오류가 존재하는 환경
    - checksum으로 헤더와 데이터의 오류 검출
    - 제어 메시지(**control message**, feedback)으로 sender에게 데이터를 잘 받았는지 못받았는지 여부 알려줌
        - 잘 받았을 때 ⇒ **ACK** 메시지
        - 오류 있을 때 ⇒ **NAK** 메시지
            - 재전송(retransmission)으로 해결
                - **ARQ**(Auto Repeat reQuest) 프로토콜 사용(자동으로 재전송하는 프로토콜)

- **Stop-and-Wait**
    - sender는 전송한 데이터에 대한 제어 메시지(ACK 또는 NAK)를 받을 때까지 응용 계층으로부터 데이터를 받지 않고 기다림
        - ACK를 받았으면 다음 데이터 전송
        - NAK를 받았으면 기다리면서 갖고 있던 데이터 재전송

![rdt2](/assets/images/rdt2.png)

<br>

1. 응용 계층으로부터 데이터 들어오면 ⇒ 오류 제어를 위한 checksum을 포함해 패킷 전송
2. Stop-and-Wait : 데이터 전송 후 제어 메시지를 기다림
3. NAK 메시지 받았으면 ⇒ 재전송
4. ACK 메시지 받았으면 ⇒ 아무 일 하지 않고 응용 계층으로부터 오는 데이터를 기다림
5. 데이터가 망가져서 왔으면 ⇒ NAK 메시지 전송
6. 받은 데이터가 정상이면 ⇒ 헤더 제거한 데이터를 응용 계층으로 보내고 + ACK 메시지 전송

<br>

- RDT 2.0의 문제점
    - **제어 메시지가 전송되는 동안 망가질 수 있다.**
        - 망가진 제어 메시지를 받은 sender는 재전송
        - 그런데 receiver는 이미 ACK 메시지를 보내고 응용 계층으로 데이터를 보낸 데이터를 또 받아도 이미 받은 데이터인지 알 수 없음
        - ⇒ **순서번호(sequence number)**로 데이터를 구분

<br>

## RDT 2.1

### sender

![rdt2_1_sender](/assets/images/rdt2_1_sender.png)

<br>

1. 0번 데이터를 전송
2. 0번 패킷에 대한 제어 메시지를 기다리고 있다.
3. 제어 메시지가 망가졌거나 NAK 메시지를 받으면 ⇒ **0번 데이터 재전송**
4. 제어 메시지 망가지지 않았고 ACK 메시지를 받으면 ⇒ 응용 계층에서 오는 데이터 기다림
5. 응용 계층으로부터 들어온 1번 데이터를 전송
6. 제어 메시가 망가졌거나 NAK 메시지를 받으면 ⇒ **1번 데이터 재전송**

<br>

### receiver

![rdt2_1_receiver](/assets/images/rdt2_1_receiver.png)

<br>

1. 데이터가 망가졌으면 ⇒ NAK 메시지 전송(재전송하기를 기다림)
2. 데이터가 망가지지 않았지만, 기대하던 번호의 데이터가 아니면(중복 데이터. 재전송을 받았을 때) ⇒ **ACK 메시지 전송**
    - 이전에 데이터 잘 받았지만, ACK 메시지가 망가졌었다는 의미
3. 데이터가 망가지지 않았고, 기대하던 번호의 데이터를 받았으면 ⇒ 응용 계층에 데이터 보내고 + ACK 메시지 전송 + 다음 데이터 번호 기다림

<br>

## RDT 2.2 : NAK-free

- NAK 메시지 없이 ACK 메시지만 전송
- receiver가 비정상적인 패킷을 받으면 NAK를 보내지 않고, **이전에 받았던 패킷에 대한 ACK**를 보낸다.
- 중복 ACK(duplicate ACK)를 받은 sender는 마지막에 보낸 데이터에 문제가 있음을 인식하고 해당 패킷을 재전송

<br>

## RDT 3.0 : with Timer

- 이전까지의 문제점
    - 데이터 또는 제어 메시지가 **손실**될 수 있다. (RDT 2.1은 데이터의 "오류" 제어)
    - ⇒ **Timer** 등장
        - 주어진 시간 동안 처리하지 못하면 재전송
        - 시간은 운영체제가 결정

### sender

![rdt3_sender](/assets/images/rdt3_sender.png)

<br>

1. 데이터를 전송한 게 없는데 데이터를 받았으면 무시
2. 응용 계층으로부터 0번 데이터를 받으면 ⇒ 패킷을 만들어 전송하고 + 타이머 시작
3. 제어 메시지가 망가졌거나 보낸 패킷에 대한 ACK가 아니면 ⇒ 아무 일도 하지 않고 타임아웃되기를 기다린다.
    - RDT 2.2에서 이전에 보낸 패킷에 대한 ACK를 보냄으로써 NAK 메시지를 대신했다면,
    - RDT 3.0에서는 타임아웃이 곧 NAK
4. 타임아웃되면 ⇒ 0번 데이터를 재전송 + 타이머 시작
5. 제어 메시지가 망가지지 않았고 보낸 패킷에 대한 ACK이면(정상적으로 전달 완료) ⇒ 타이머 종료 + 응용 계층에서 요청하는 다음 데이터 기다림

<br>

### receiver

![rdt3_receiver](/assets/images/rdt3_receiver.png)

<br>

1. 기다리던 패킷이 아니면(보낸 ACK 메시지가 망가짐) ⇒ 받은 패킷에 대해 ACK 전송
2. 데이터가 망가졌으면 ⇒ 아무 일도 하지 않음. sender가 타임아웃되고 재전송 할 것(sender 입장에서 데이터 손실)
3. 정상적인 데이터를 받았으면 응용 계층으로 데이터를 보내고 sender에게 ACK 메시지 전송

<br>

## RDT 3.0 동작

[rdt3](https://t4.daumcdn.net/thumb/R720x0/?fname=http://t1.daumcdn.net/brunch/service/user/1dLN/image/D49UXZhszT6mMWPyq0q_6omvROw.gif)

<br>

a. 손실 없이 정상 작동

b. sender가 전송한 패킷이 손실됐을 때 ⇒ 타임아웃에 의해 패킷 재전송

c. 제어 메시지가 손실됐을 때 ⇒ 타임아웃에 의해 패킷 재전송

d. 타이머의 시간이 짧았거나, ACK 메시지가 늦게 도착했을 때 ⇒ sender는 타임아웃 됐던 패킷 재전송, receiver는 ACK를 보냈던 패킷에 대해 다시 ACK 전송