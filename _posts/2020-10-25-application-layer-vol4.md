---
layout: post
title: "HTTP 메시지 구조"
author: "DAEUN"
---

## HTTP 메시지

1. request message
2. response message
- ASCII로 작성되어 보안에 취약 → HTTP 2.0 이후 바이너리로 전환

<br>

## Request Message 구조

![request_msg](/assets/images/request_msg.PNG)

<br>

- request line + header lines + body
- **request line**
    - method(GET, POST, PUT 등) + URL(상대주소) + http version
- header line
    - host: 서버 호스트 url
    - connection: 연결 방법(non-persistent / persistent)
    - user-agent: 브라우저의 종류 및 버전
    - accept: 원하는 콘텐츠의 특징(파일 포맷, 언어 등)
- body
    - 서버에 전달할 내용
    - GET 메소드에는 body 없음

<br>

## Response Message 구조

![response_msg](/assets/images/response_msg.PNG)

<br>

- status line + header lines + data
- **status line**
    - protocol version + 상태 코드(200, 404 등) + 상태 설명(OK, Not Found 등)
- header line
    - date: 서버가 응답 메시지 만든 시간
    - server: 서버의 종류 및 버전
