---
layout: post
title: "응용 계층의 프로토콜 : FTP"
author: "DAEUN"
---

## FTP

- File Transfer Protocol
- TCP 연결이 두 개
    - control (인증, 디렉토리 변경, put, get 등)
    - data
- 동작
    1. TCP 연결 : **control** (포트 번호: 21)
    2. ID/PW 확인
    3. TCP 연결 : **data** (포트 번호: 20)
    4. 파일 전송
        - control 연결은 유지한 채, 하나의 파일을 전송할때마다 data 연결
        - non-persistent

<br>

### HTTP vs FTP

- 공통점
    - **파일 전송** 프로토콜
    - **TCP** 연결 기반
- 차이점
    - HTTP: **하나**의 TCP 연결
    - FTP: **두 개**의 TCP 연결
