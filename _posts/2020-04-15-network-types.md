---
layout: post
title: "Network Types"
author: "DAEUN"
---

### Local Area Network(LAN)
LAN은 사무실, 건물, 캠퍼스와 같이 비교적 좁은 범위 내에서 서로 다른 호스트를 연결한 네트워크이다. 과거에는 호스트를 모두 하나의 공통된 케이블에 연결했지만 오늘날은 스위치에 각각 연결해 한 번에 여러 대가 통신할 수 있고 트래픽이 완화됐으며 패킷(packet)에 적힌 주소로 바로 보내지기 때문에 다른 호스트들이 자신에게 온 데이터인지 식별할 필요가 없어졌다. 보통 LAN으로 좁은 범위의 네트워크를 형성하고 다른 지역과 통신하기 위해 서로 다른 LAN을 WAN으로 통신한다.
<br><br>
![LAN](/assets/images/LAN.PNG)
<br><br><br>
### Wide Area Network(WAN)
WAN은 LAN 보다 더 넓은 범위에서 네트워크를 형성하는 것으로, 도시, 국가, 전세계까지 그 범위를 넓힐 수 있다. 스위치(switch), 라우터(router), 모뎀(modem)을 연결하며 보통 통신사에 의해 제공된다. 오늘날 WAN은 point-to-point WAN 그리고 switched WAN이 있다.
<br><br>
![point to point WAN](/assets/images/point_to_point_WAN.PNG)<br>
point-to-point WAN은 서로 다른 네트워크를 연결하고 있는 두 기기를 연결한 네트워크를 말한다.
<br><br>
![switched WAN](/assets/images/switched_WAN.PNG)<br>
switched WAN은 몇 개의 point-to-point WAN이 스위치로 연결된 네트워크를 말하며 오늘날 전세계 네트워크를 연결할 때 쓰이는 중심 네트워크로 쓰이고 있다.
<br><br><br>
### Internetwork
LAN과 WAN은 단독으로 쓰이는 일이 거의 없고 서로 연결되어 네트워크를 형성한다. 두 개 이상의 네트워크가 서로 연결되어 있을 때, 이를 internetwork라고 부르며 internet이라고도 한다(소문자 i임에 주의).
<br><br>
![internetwork](/assets/images/internetwork.PNG)<br>
우리나라 서울시에 있는 어느 사무실에서 중국 상해에 있는 사무실과 통신할 경우 각 사무실은 자신들의 LAN에 속해 있고 이 두 네트워크를 WAN이 연결한 형태를 띈다. 만약 서울시의 사무실에서 같은 건물에 있는 다른 호스트에게 메시지를 보낸다면 라우터는 메시지를 block하고 스위치가 메시지가 가야할 곳으로 보낸다. 서울시의 사무실에서 상해의 사무실에 메시지를 보낸다면 라우터 R2가 R2로 packet을 전송하여 목적지로 전달된다.
<br><br>
![internetwork2](/assets/images/internetwork2.PNG)<br>
LAN과 WAN이 결합된 또 다른 네트워크 형태이다. switched WAN이 중심 역할을 하고 있는 것을 볼 수 있다.
<br><br><br>
reference: Behrouz A. Forouzan, _Data Communications and Networking_, 5th ed. New York: McGraw-Hill, 2012.