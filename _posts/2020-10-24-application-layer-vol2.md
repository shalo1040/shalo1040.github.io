---
layout: post
title: "응용 계층의 프로토콜 : HTTP"
author: "DAEUN"
---

### 웹

- 최초의 인터넷 app. killer application
- Client-Server 구조
- 그래픽 기반의 콘텐츠
- 웹 페이지는 **객체(object)**로 구성됨
- **URL**(Uniform/Universal Resource Locator)
    - 객체가 저장되어 있는 위치 가리킴
    - host name + path name
    - www.naver.com/index.html

<br>

### HTTP

- Hypertext Transfer Protocol
- client program 예: Chrome 브라우저
- server program 예: Apache
- HTTP가 하는 일
    1. **메시지**(request, response message)를 **구조화**
    2. 메시지를 클라이언트와 서버 간 **교환**
    3. 사용자가 원하는 **화면**을 보여줌
- 전송 계층에서 **TCP 프로토콜** 사용 ⇒ 텍스트 기반이므로 **신뢰성** 있는 통신이 필요하기 때문
    - TCP socket 생성(포트번호는 80 ← well-known)
    - 세그먼트(= TCP 헤더 + 메시지) 전달
- 클라이언트가 데이터를 요청했을 때만 서버가 응답
- **Stateless Protocol**
    - 클라이언트의 정보(IP 주소, 프로세스 정보, 요청 내용 등)을 서버에 저장하지 않음
- **pull 방식의 프로토콜**
    - 사용자가 요청한 데이터를 pull하여 가져오는 형식이기 때문
- 데이터 손실, 복구 등에 관여하지 않음
    - 하위 계층에서 해야할 일(TCP의 일)
    - 하위 계층으로부터 완벽한 데이터를 가져왔다는 전제하에 작업
- 연결 방법: [non-persistent HTTP, persistent HTTP](/2020-10-24/application-layer-vol3)