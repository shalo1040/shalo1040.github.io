---
layout : post
title: "응용 계층의 프로토콜 : SMTP"
author: "DAEUN"
---

- Simple Mail Transfer Protocol
    - mail server 간 메일 주고받을 때 사용
        - 브라우저를 통해 메일을 주고받는다면 SMTP가 아닌 HTTP 사용
    - 전달 실패 시, 주기적으로 다시 시도하다가 폐기
- user agent: 메일 앱(outlook, iPhone mail 등)
- mail server
    - message queue에 수신자에게 보낼 메일 넣는다.
    - mailbox에 유저에게 전송된 메일을 넣는다.

![smtp](/assets/images/smtp.PNG)

1. Alice가 Bob에게 메일을 전송하는 버튼을 누른다.
2. user agent가 mail server의 message queue에 메일 전달
    - user agent를 통해 메일을 보낼 경우, SMTP 사용(push 형식)
    - 웹 상에서 메일을 쓰고 보낼 경우, HTTP 사용
3. 메일 전달을 위해 Bob의 mail server와 TCP 연결(포트 번호 25)
4. handshaking을 통해 초기 연결 후, 메일 전송
    - **SMTP** 프로토콜 사용(push 형식)
    - user agent 사용 ⇒ SMTP over TCP connection
    - 브라우저 사용 ⇒ HTTP over TCP connection
5. 메일을 받아 사용자 메일함에 저장
6. Bob이 메일을 읽음
    - SMTP 사용하지 않음
    - POP3, IMAP, HTTP 등 프로토콜 사용(pull 형식)