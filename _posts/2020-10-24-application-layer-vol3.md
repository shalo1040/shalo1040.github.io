---
layout: post
title: "non-persistent HTTP vs persistent HTTP"
author: "DAEUN"
---

### non-persistent HTTP

- **한 번의 연결** + **한 개의 객체** 전송
    - 한 번의 연결: **3-way-handshaking**
        1. 서버에게 연결해도 되는지 여부 물어본다.
        2. 1에 대한 서버의 응답
        3. 2에 대한 클라이언트의 응답
    - 만약 html에 10개의 image 객체가 있다면?
        - html 파일(객체) 한 개 + 10개의 image 객체 ⇒ 11번의 TCP 연결 필요

![non_persistent](/assets/images/non_persistent.PNG)

<br>

위와 같이 1\~6을 객체 수만큼 반복하는 것은 비효율적임 ⇒ persistent HTTP 필요성

<br>

<img src="/assets/images/rtt.PNG" width="300">

- **RTT** : Round Trip Time
    - 클라이언트가 서버에게 요청한 시각부터 응답을 받는 데까지 걸린 시간
- non-persistent HTTP의 소요시간
    - **2RRT + 파일 전송 시간**
    - 첫 번째 RRT: TCP 연결
    - 두 번째 RRT: 데이터 요청 및 응답
- 모든 객체마다 2RRT만큼 추가 시간 소요
    ⇒ OS의 오버헤드
    ⇒ 비효율적

<br>

### persistent HTTP

- pipelining 이라고도 함
- **한 번의 연결** + **다수의 객체** 전송