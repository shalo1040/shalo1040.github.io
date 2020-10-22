---
layout: post
title: "네트워크 문제점과 성능"
author: "DAEUN"
---

# 네트워크 문제점

---

<br>

## 패킷 손실(Packet Loss)

![packet loss](/assets/images/packet_loss.PNG)

라우터의 버퍼(큐)는 유한해서 버퍼가 모두 차 있을 때 새로운 패킷은 들어오지 못하고 손실된다. (Packet Error Rate, PER) 데이터가 손실되면 재전송을 하며 그만큼 시간이 지연된다.

<br>

## 지연(Delay)

![delay](/assets/images/delay.PNG)

<br>

(전체 지연 시간) = (processing delay) + (queueing delay) + (transmission delay) + (propagation delay)

**processing delay**: 라우터가 패킷을 또 다른 기기로 보내는 결정을 하는 데 걸리는 시간(=switching 하는 시간=routing(라우터에서 하는 일) 시간)

**queueing delay**: 전송 대기 큐(output queue)에서 기다리는 시간. 라우터의 혼잡도에 따라 영향을 받는다.

**transmission delay**: 패킷을 전송하는 데 걸리는 시간. 패킷의 길이가 길수록 전송에 더 많은 시간이 걸린다.   ${packet-length \above{1pt} link-bandwidth}$

**propagation delay**: 노드에서 노드로 이동하는 데 걸리는 시간. 링크의 길이가 길수록 시간은 오래 걸린다. 링크의 종류에 따라서도 이동 속도가 달라진다. (ex. 구리선 > 광선)  ${length-of-physical-link \above{1pt} propagation-speed}$

<br>

### 혼잡(Congestion)

회선이 수용할 수 있는 것보다 많은 양의 데이터가 들어오게 되면 중계기에서 혼잡이 발생한다. 이는 패킷 손실 문제로 이어지며 문제를 해결하기 위해 재전송을 하면서 시간 지연이 발생한다.

<br>

# 네트워크 성능

---

<br>

처리량은 높고 지연은 낮을수록 효율적인 네트워크이다.

- **처리량(throughput)** : 일정한 시간동안 처리한 일의 양

![throughput](/assets/images/throughput.PNG)

<br>

- **지연(delay)** : 전달시간, 응답시간 등. 사용자 수, 전송매체의 종류, 소프트웨어 및 하드웨어의 성능 등 복합적인 요인에 의해 발생한다.

- **전송속도** : 단위 시간 당 전송되는 데이터의 양.  ${데이터전송량 \above{1pt} 단위시간}$ bps (bits per second)

- **대역폭** : 단위 시간 당 전송될 수 있는 최대 데이터의 양. 도로의 차선에 비유.  ${최대데이터전송량 \above{1pt} 단위시간}$ bps

<br>

## 네트워크 관련 표준화 기관들

ITU (International Telecommunication Union)

ISO (International Organization for Standardization)

ANSI (American National Standard Institute)

IETF (Internet Engineering Task Force)

등등