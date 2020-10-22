---
layout: post
title: "Program vs Process vs Thread"
author: "DAEUN"
---

### 프로그램(Program)
: 하드디스크에 설치되어 있는 소프트웨어(정적)

<br>

### 프로세스(Process)
: 메모리에 적재(load)되어 현재 실행 중인 프로그램(동적)

* 한 프로그램에서 실행되는 여러 프로세스 동시 존재 가능. 예) 웹 브라우저의 탭.
* 순차적 진행
* new, ready, running, waiting, terminated의 상태를 가짐
* 프로세스 구성: text(프로그램 코드), data(전역 변수), heap(동적 할당 메모리 영역), stack(지역 변수, 함수 매개 변수, 복귀 주소)
* 커널의 메모리에 프로세스의 정보를 PCB(Process Control Block)에 저장해 운영체제가 process 관리
* PCB 구성: 프로세스 상태, Program Counter(PC), CPU 레지스터 사용 정보, 사용된 CPU 양, 실행 시작 이후 경과된 클럭 타임 등

[더 자세한 내용](https://shalo1040.github.io/2020-02-08/process)

<br>

### 스레드(Threads)
: CPU 이용의 기본 단위. 프로세스 내에서 실행되는 흐름, 세부 작업 단위.

예1) 워드 프로세서에서 글자를 입력하면서(키보드 입력 받는 스레드) 화면에 내용이 보여지고(그래픽 표시 스레드) 동시에 맞춤법 검사(철자 체크 스레드)를 한다.
예2) 웹 서버는 클라이언트의 요청을 listen하는 스레드를 생성하고, 요청이 들어오면 이를 서비스 할 새로운 스레드 생성(다중스레드 서버 아키텍처)

* 스레드는 각각 별도의 프로시저를 호출하고 동작의 흐름을 갖기에 stack을 갖는다. 이 외에도 각자 스레드ID, 프로그램 카운터(PC), 레지스터 집합을 갖는다. 스레드는 독립돼있지 않기 때문에 프로세스의 code, data, OS resources(open files and signals)를 다른 스레드와 공유한다.

[스레드 장점]
1. **응답성**(Responsiveness): 프로세스 일부가 블록되더라도 실행 가능. 사용자 인터페이스 기능에 유용
2. **자원 공유**(Resource Sharing): 프로세스의 자원을 공유(프로세스 간 통신(IPC)보다 비용이 적음) => but, **동기화 문제 주의 필요**
3. **경제성**(Economy): 스레드 생성, 문맥교환 시간이 프로세스보다 적음(문맥교환->시스템 콜->오버헤드 적을수록 좋음)
4. **확장성**(Scalability): 다중처리기 아키텍처(multicore/multiprocessor)의 이점(병렬 수행) 활용 가능

=> 멀티프로세스보다 멀티스레드가 유리

<br>

| 프로세스 | 스레드 |
| :---: | :---: |
| **운영체제로부터 메모리 자원을 할당 받는다.** | **같은 프로세스에 있는 스레드들은 프로세스의 자원(code, data, heap)을 공유한다.** |
| 문맥교환 시간이 **길다**. | 문맥교환 시간이 **적다**. |
| 프로세스 간의 통신 => **비효율적** | 스레드 간의 통신 => **효율적** |
| creation과 terminate 시간이 **길다**. | creation과 terminate 시간이 **짧다**. |

<br><br>

References : [프로세스와 스레드의 차이1](https://brunch.co.kr/@kd4/3), [프로세스와 스레드의 차이2](https://www.guru99.com/difference-between-process-and-thread.html), [프로세스와 스레드의 차이3](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-os.html)