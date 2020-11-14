---
layout: post
title: "응용 계층의 프로토콜 : DNS"
author: "DAEUN"
---

- Domain Name System
- hostname으로 request 보낸 사용자의 요청을 네트워크 장비가 이해할 수 있는 IP 주소로 바꿔준다.
- **UDP** 기반 (포트 번호 : 53)

    ⇒ 빨리 매칭되는 IP 주소를 get 해야 함

- 응용 계층 프로토콜(HTTP, FTP, SMTP 등)을 도와주는 역할
- 구조
    - 중앙 집중 구조(centralized design)
        - 하나의 DNS 서버가 모든 정보를 가지고 있는 구조
        - 단점
            - 트래픽 폭주
            - 정보 유지 문제
            - 서버와의 거리에 따른 시간 지연 문제 발생
            - 서버 고장 시, 전체 마비
    - **분산/계층적 구조**
        - 서버를 분산시킨 구조. 각 서버는 url의 일부를 읽고 관련 정보를 갖고 있는 서버를 안내한다.
        - 매번 같은 url에 대해 IP 주소를 찾게 되면 비효율적이므로 IP 주소를 저장해두는 **DNS caching**으로 해결할 수 있다.
        - **Root** 서버: TLD 정보 알려줌
        - **Top-level domain(TLD)** 서버
            - com, net, edu, gov, kr, uk 등
        - **Authoritative** 서버: 기관 DNS 서버