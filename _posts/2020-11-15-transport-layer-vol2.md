---
layout : post
title : "UDP"
author : "DAEUN"
---

- 전송계층의 대표적 기능인 **multiplexing**과 **demultiplexing**만 하는 프로토콜
- connectionless(no handshaking)
- 단점
    - 신뢰성 보장 못함
        - 세그먼트 손실 가능
        - 세그먼트의 순서 및 정렬 기능 없음
- 장점
    - 처리속도가 빠르다.
        - 연결 설정에 의한 지연에 민감한 경우 유용
            - 예) DNS
        - 연결 상태를 저장할 필요 없는 경우 유용
            - 예) DNS, [RIP](https://www.geeksforgeeks.org/routing-information-protocol-rip/), [SNMP](http://www.ktword.co.kr/abbr_view.php?m_temp1=279)
    - 적은 오버헤드
        - UDP는 헤더의 크기가 작다.
        - 반면, TCP는 헤더에 정보가 더 많고 해야할 일도 많다.

<br>

### UDP 헤더 구조

![udp header structure](https://images2018.cnblogs.com/blog/1126979/201806/1126979-20180619144725146-196436604.png)

<br>

- source port # : 보낸 PC의 프로세스 번호
- dest port # : 받는 PC의 프로세스 번호
- length : 세그먼트의 길이(헤더 + 데이터 길이)
- checksum : 헤더 오류 검사
    - checksum 구하는 방법
        1. 1의 보수를 취한다. (0과 1 전환)
        2. source port #, dest port #, length를 모두 더한다. (오버플로우 무시)
    - 헤더 오류 검사 방법
        1. source port #, dest port #, length, checksum을 모두 더한다. (오버플로우 무시)
        2. 모든 비트가 1이면 정상, 0이 존재하면 에러
    - 에러 발생 시 세그먼트 버림