---
layout: post
title: "웹 기술 : Cookie, Web Caching"
author: "DAEUN"
---

## Cookie

- HTTP는 stateless protocol로, 클라이언트의 정보를 서버가 저장하지 않음 ⇒ 클라이언트 정보를 저장하기 위한 데이터 파일인 cookie 등장
- 목적
    - 사용자 신분 확인
        - 자동 로그인
    - 사용자 정보 저장
        - 방문 페이지 및 상태 저장, 마케팅
- 종류
    - **Session Cookie**
        - 메모리에만 저장
        - 브라우저가 종료되면 삭제
    - **Persistent Cookie**
        - 클라이언트의 하드디스크에 저장 ⇒ 보안 취약
        - 일정 기간이 지나면 만료되어 참고하지 않음

<br>

## Web Caching

<img src="/assets/images/web_caching.PNG" width="300">

<br>

- **proxy server**: origin server를 대신해 요청 메시지를 처리하는 일종의 미러 서버
- 동작
    1. proxy server에 데이터 요청

    2-1. 데이터 있을 경우 클라이언트에 반환

    2-2. 데이터 없을 경우 origin server에 요청

- 장점
    - **응답시간**을 줄일 수 있다.
    - **트래픽**을 줄일 수 있다.
