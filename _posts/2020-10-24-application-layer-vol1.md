---
layout: post
title: "Application Architecture: Client-Server, P2P"
author: "DAEUN"
---

응용서비스 개발자: 네트워크 구조(network architecture) 신경쓰지 않아도 되지만, **응용서비스의 구조(application architecture)** 결정해야함

<br>

### Client-Server

<img src="/assets/images/client_server.PNG" width="400">

<br>

예) 사용자가 네이버에서 검색하면 네이버 서버에서 적절한 결과를 사용자에게 반환한다.

- Server
    - always-on host. 항상 작동 중이며 여러 client의 요청을 받는다.
    - 고정적이고 영구적인 IP 주소
    - 데이터 센터(mirror server) : 하나의 서버만으로는 너무 많은 요청을 처리하기 어렵다. 따라서 여러 대의 서버를 둠으로써 일을 나눈다. (구글의 경우 전세계에 30\~50개의 서버가 존재한다고..)

<br>

- Client
    - 서버와 간헐적 연결
    - 고정적 또는 동적 IP 주소(사용자는 집, 카페 등 위치를 변경하며 일을 해도 됨)
    - 다른 클라이언트(end system)와 직접적으로 통신하지 않는다.

<br>

### P2P(Peer to Peer)

<img src="/assets/images/peer_to_peer.PNG" width="400">

<br>

- 하나의 peer가 서버 또는 클라이언트 모두 될 수 있다.
- peer끼리 통신한다 ⇒ Client-Server 구조보다 서버의 필요성이 적으므로 **비용 절약** 가능
    - 서버의 도움을 받지 않는 통신 방법
        - 하나의 파일을 여러 peer가 chunk 단위로 전송 가능
    - 서버의 도움을 받는 통신 방법
        - 파일을 받고자하는 peer가 tracker(서버)에게 이를 요청하면 tracker는 해당 파일을 갖고 있는 peer를 안내해줌. 해당 peer를 통해 파일 다운
- **self scalability(자기 확장성)**: P2P에 연결된 기기가 많을수록 자원이 많아지는 점에서 확장성이 있다고한다. 예를 들어, 각 peer가 파일을 요청해 처리해야 할 업무가 많더라도, 다른 peer에서 파일을 전송해 해결할 수 있다.

    [scalability 뜻](https://www.techopedia.com/definition/9269/scalability)

- peer끼리 간헐적 연결. 즉, IP 변경 가능(단, 파일 전송중일 때는 고정 IP)
- 단점: 보안이 약함, 관리 복잡

<br>

![client_server_p2p](/assets/images/client_server_p2p.PNG)

<br><br>

References: [client-server vs p2p](https://www.veterinaryitsupport.com/peer-to-peer-vs-client-server-networks/), [client-server vs p2p](https://www.bbc.co.uk/bitesize/guides/zh4whyc/revision/7), [p2p](https://hojak99.tistory.com/460)