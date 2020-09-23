---
layout: post
title: "정보 전송 방식: 회선교환 vs 패킷교환"
author: "DAEUN"
---

## 회선 교환(Circuit Switching)

![https://i.pinimg.com/originals/67/0a/8a/670a8aa9cbf5e66d1323aeed3955fe79.gif](https://i.pinimg.com/originals/67/0a/8a/670a8aa9cbf5e66d1323aeed3955fe79.gif)

<br>

단말기 사이 회선을 연결하고 두 end system 간 통신할 때는 회선을 마치 **전용선인 것처럼** 사용하는 방식을 말한다. 회선을 점유하고 있는 동안 다른 단말기가 그 회선을 사용할 수 없다. 이와 같은 방식은 데이터를 **안정적**으로 오래 보내야하는 **전화망**에 사용한다.

<br>

![graph](/assets/images/switching_graph.png)

<br>

공유선을 마치 전용선처럼 사용하며 안정적인 통신을 할 수 있다는 장점이 있지만, 인터넷망에는 **비효율적**이다. 전화와 같은 경우, 처음부터 끝까지 음성 데이터를 끊김없이 보내야하기 때문에 회선 교환 방식이 적절하지만, 인터넷의 경우 데이터를 통신하는 순간은 서버에 웹을 요청하는 등의 짧은 순간이고 여러 웹사이트를 동시에 접속할 수 있어야하기 때문이다. 따라서 인터넷에는 회선 교환 방식이 아닌 패킷 교환 방식을 사용한다.

<br><br>

## 패킷 교환(Packet Switching)

![https://www.rfwireless-world.com/images/circuit-switching-vs-packet-switching-fig2.jpg](https://www.rfwireless-world.com/images/circuit-switching-vs-packet-switching-fig2.jpg)

<br>

패킷 교환 방식은 여러 단말기가 회선을 **공유**하며 데이터를 **패킷(packet)**이라는 단위로 나누어 전송하는 것을 말한다. 이를 통해 **효율적**으로 회선을 사용할 수 있게 되었다. 하지만 동시에 너무 많은 단말기가 동일한 회선을 공유한다면 **서비스 지연** 및 **지터(jitter)**가 발생할 수 있다는 단점도 존재한다. 그러나 인터넷망에서는 회선의 효율적 사용을 더 중요한 가치로 생각하기 때문에 패킷 교환 방식을 채택하며, 지터의 발생을 최소화하기 위해 버퍼링과 같이 미리 일정 데이터를 받아왔을 때부터 콘텐츠를 사용자에게 보여주는 방법을 사용한다.

<br>

```
TV를 시청할 때 위성안테나, 셋톱박스, 유튜브 라이브 방송 순으로 전송속도가 빠르다.
```