---
layout: post
title: "Introduction to Data Communication"
author: "DAEUN"
---

### 데이터 통신의 구성요소
![five components](/assets/images/five_components.PNG)<br>
data communication의 다섯가지 구성요소는 message, sender, receiver, transmission medium, protocol.

### data flow
![data flow](/assets/images/data_flow.PNG)<br>

### 좋은 네트워크란?
성능이 좋고(Performance), 신뢰성이 높고(Reliability), 보안이 우수한(Security) 네트워크를 좋은 네트워크라고 볼 수 있다.

**Performance**: 많은 throughput(처리량), 짧은 delay(지연 시간)를 가진 네트워크일수록 좋은 네트워크다.
**Reliability**: 오류가 잦지 않고 오류가 나더라도 빠른 회복 가능한 네트워크가 좋은 네트워크다.
**Security**: 외부로부터의 침입을 막고 데이터를 보호하고 데이터 손실이 일어났을 때 복구를 잘하는 네트워크가 좋은 네트워크다.

### type of connection
![types of connection](/assets/images/types_of_connections.PNG)<br>
**Point-to-Point**: 두 기기 간 직접적으로 통신할 수 있게 연결하는 네트워크를 말한다. 컴퓨터와 프리터의 연결처럼 물리적으로 케이블로 연결할 수 있고 적외선 리모콘으로 TV를 조작하는 방법으로 통신할 수도 있다.<br>
**Multipoint**(Multidrop): 하나의 선을 다수의 기기가 공유하는 형태로 연결하는 네트워크를 말한다. 이 연결 형태를 사용하면 송수신할 때 링크에 있는 모든 기기가 데이터를 받을 수 있다.

### 토폴로지
네트워크 구성 요소인 노드와 링크가 배치되는 방법을 토폴로지라고 말하며 메쉬 형태, 스타 형태, 버스 형태, 그리고 링 형태가 있다.

![mesh topology](/assets/images/mesh_topology.PNG)<br>
**Mesh Topology**(메쉬 형태, 그물 형태, 망 형태): 모든 기기들 간 **Point-to-Point** 연결이 되어 서로 그물처럼 얽혀 있는 형태를 말한다. 1:1로 직접적인 통신이 가능하기 때문에 링크를 공유했을 때 발생하는 트래픽 문제가 덜 발생하고 데이터도 보호할 수 있다. 또 이 형태는 하나의 링크가 고장이 나도 전체 시스템에 영향을 주지 않는다는 점에서 튼튼하다고할 수 있다. 그러나 기기를 서로 연결하기 위해 수많은 케이블(_n*(n-1)/2_)이 필요하며 각 기기마다 I/O 입출력 포트가 필요하다. 이 때문에 메쉬 형태로 연결하는 것은 설치 비용과 유지 비용이 많이 들어 다른 토폴로지와 결합하여 그들을 연결해줄 때 사용된다.

![star topology](/assets/images/star_topology.PNG)<br>
**Star Topology**(스타 형태, 성 형태): 기기들끼리 서로 연결된 메쉬 형태와 달리 각 기기들은 hub라고 불리는 중앙 처리 장치에 point-to-point로 연결된다. 따라서 정보를 보내고 받기 위해서 이 허브를 통해야만한다. 스타 토폴로지는 각 기기마다 하나의 링크와 입출력 포트가 필요해 메쉬 토폴로지보다 비용이 덜 든다(그러나 뒤에 나오는 버스형과 링형에 비하면 많은 케이블을 사용한다). 이러한 이유 때문에 설치와 유지비용이 감소하며 하나의 링크가 고장이 나더라도 다른 기기에 영향을 주지 않는다는 장점이 있다. 하지만, 이 토폴로지는 허브에 의존적이기 때문에 허브가 고장이 난다면 전체 시스템이 작동하지 않는다. 스타 형태는 LAN에 사용된다.

![bus topology](/assets/images/bus_topology.PNG)<br>
**Bus Topology**(버스 형태): 앞서 살펴본 메쉬 토폴로지와 스타 토폴로지와 달리 멀티포인트 연결 형태를 띈다. 공유된 하나의 케이블에 연결하기 위해 T커넥터(tap)로 기기와 연결한다. 하나의 버스 케이블을 제외하고 기기는 가장 가까운 위치로 연결하면 되기 때문에 메쉬와 스타 토폴로지보다 필요한 케이블의 길이가 짧다고 볼 수 있어 설치가 간편하다. 하지만 버스 토폴로지는 처음 설치할 때 당시 최적으로 설치하기 때문에 새로운 기기를 연결하려면 케이블 위치 등을 수정할 필요가 있다. 또한, 버스 케이블에 전기 신호가 흐르면서 열에너지로 변환이 되어 멀리 갈수록 점점 힘이 약해진다. 버스 케이블 또는 기기에 문제가 발생했다면 전체 시스템이 마비가 되어 결함고립(fault isolation)이 어렵다. 이 토폴로지는 초기 LAN 사용 당시 사용되었다.

![ring topology](/assets/images/ring_topology.PNG)<br>
**Ring Topology**(링 형): 양 옆에 있는 기기와 point-to-point 연결이 된 형태를 띈다. 이 링 형에서는 목표 지점에 도달할 때까지 신호가 한쪽 방향으로 흐른다. 모든 기기 연결 지점에는 repeater가 존재해 해당 기기로 온 신호가 아니라면 비트를 재생산해 다른 기기로 갈 수 있도록 한다. 일정 시간이 지나도 기기가 신호를 받지 못하면 네트워크 관리자에게 관련 정보를 알려주는 알람을 준다. 모든 기기는 양 옆의 기기와 두 개의 연결만을 가지고 있어 새로운 기기를 설치하고 삭제하기 쉽다. 그러나 신호가 한쪽 방향으로만 흘러 링의 일부분이 고장난다면 전체 시스템이 작동되지 않는데, 이를 해결하기 위해 두 개의 링을 사용하거나 고장난 기기를 꺼버리는 스위치를 둠으로써 해결할 수 있다. 링형은 IBM이 LAN을 발표했을 때 많이 사용됐다.
<br><br><br>
reference: Behrouz A. Forouzan, _Data Communications and Networking_, 5th ed. New York: McGraw-Hill, 2012.