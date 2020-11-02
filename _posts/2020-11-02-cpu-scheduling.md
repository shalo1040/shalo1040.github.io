---
layout : post
title : "CPU 스케줄링"
author : "DAEUN"
---

### CPU 스케줄링 결정이 발생하는 4가지 상황

1. running -> waiting
	- I/O request, wait()
2. running -> ready
	- interrupt
3. waiting -> ready
	- I/O 완료
4. terminated

<br>

- 1, 4번: 비선점 방식(nonpreemptive 또는 cooperative)
	- 프로세스가 종료되거나 문맥 교환이 일어나기 전까지 CPU를 양보하지 않음
- 2, 3번: 선점 방식(preemptive)
	- CPU를 사용하지 않고 있으면 다른 프로세스가 사용
	- 공유 자원에 대한 경쟁 상황 발생할 수 있음
	- 커널 모드에 있는 동안 선점을 당하거나 작업 중 인터럽트 발생하는 경우도 고려 필요

<br>

### Dispatcher

- 단기 스케줄러가 선택한 프로세스에게 CPU 제어권을 주는 모듈
	1. 문맥 교환
	2. 사용자 모드로 전환
	3. 프로그램을 다시 시작하기 위해 사용자 프로그램의 올바른 위치로 jump

- 문맥 교환동안 일어나므로 매우 빨리 동작해야 함
	- dispatch latency(디스패치 지연)
		- 하나의 프로세스를 종료하고 다른 프로세스의 실행을 시작하는 데까지 걸리는 시간

<br>

### 스케줄링 기준

- CPU 이용률(utilization)
	- 최대한 CPU를 바쁘게 해야함(40~90%)
- 처리량(throughput)
	- 단위 시간당 완료된 프로세스의 개수
- 총 처리 시간(turnaround time)
	- 프로세스를 실행하는 데 소요된 총 시간
	- 메모리 대기 시간 + 준비 완료 큐 대기 시간 + CPU 시간 + 입출력 시간
- 대기 시간(waiting time)
	- ready 큐에서 대기한 총 시간
- 응답 시간(response time)
	- 프로세스를 요청한 때부터 첫 응답이 나올 때까지 걸린 시간

<br>

## First-Come, First-Served(FCFS) 스케줄링

시스템에 들어온 순서대로 실행

| Process | Burst Time |
| :---: | :---: |
| P1 | 24 |
| P2 | 3 |
| P3 | 3 |

<br>

위와 같이 P1, P2, P3 순서대로 시스템에 도착했을 때 간트 차트(Gantt Chart)는 아래와 같다.

<br>

![fcfs](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_FCFS_Chart1.jpg)


- 대기 시간
	- P1 = 0
	- P2 = 24
	- P3 = 27
- 평균 대기 시간 = (0+24+27)/3 = **17**

<br><br>

반면, 만약 프로세스가 P2, P3, P1 순서로 들어왔다면 아래와 같을 것이다.

<br>

![fcfs](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_FCFS_Chart2.jpg)


- 대기 시간
	- P1 = 6
	- P2 = 0
	- P3 = 3
- 평균 대기 시간 = (6+0+3)/3 = **3**

<br>

=> 앞서 본 경우보다 성능이 향상됨을 알 수 있다.<br>
=> **호위 효과(Convoy effect)** : CPU를 오래 사용하는 프로세스를 다른 모든 프로세스들이 기다리며 겪는 효과<br>
예) CPU-bound 프로세스를 실행하는 동안 I/O-bound 프로세스는 CPU를 오래 사용하지 않을 것임에도 기다려야하며 I/O 장치도 일하지 않고 있는다.<br>
=> **CPU와 장치 이용률 저하**

<br>

## Shortest-Job-First(SJF) 스케줄링

ready 큐에 있는 프로세스의 burst 길이가 작은 프로세스를 스케줄

- 최소의 평균 대기 시간을 보장한다는 측면에서 최적의 스케줄러
- 하지만 다음 CPU burst를 알아내는 방법이 어려움
	- 장기 스케줄러(disk->memory)
		- 사용자가 프로세스 실행을 요청할 때 명시한 시간 사용
	- 단기 스케줄러(memory->CPU)
		- 다음 프로세스의 CPU 사용 시간을 알지 못함
		- 이전에 실행했던 프로세스와 비슷할 것으로 가정하고 근사값이 가장 짧은 CPU burst를 가진 프로세스 선택
			- 지수평균(exponential average) 사용
			- estimate[ i + 1 ] = alpha * burst[ i ] + ( 1.0 - alpha ) * estimate[ i ]
			- 0 <= alpha <= 1

<br>

| Process | Burst Time |
| :---: | :---: |
| P1 | 6 |
| P2 | 8 |
| P3 | 7 |
| P4 | 3 |

<br>

위와 같이 프로세스가 존재할 때, 간트 차트(Gantt Chart)는 아래와 같다.

<br>

![sjf](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_SJF_Chart.jpg)


- 평균 대기 시간 = (3+16+9+0)/4 = 7
	- FCFS 스케줄링일 때의 평균 대기 시간 = 10.25(ms)

<br>

선점 가능한 SJF는 다음에 나오는 Shortest-Remaining-Time-First이다.

<br>

## Shortest-Remaining-Time-First(최단 잔여 시간 우선) 스케줄링

SJF 스케줄링에서 프로세스의 도착 시간에 따라 CPUT burst가 가장 작은 프로세스 선택

<br>

| Process | Arrival Time | Burst Time |
| :---: | :---: | :---: |
| P1 | 0 | 8 |
| P2 | 1 | 4 |
| P3 | 2 | 9 |
| P4 | 3 | 5 |

<br>

위와 같이 프로세스가 요청될 때, 간트 차트(Gantt Chart)는 아래와 같다.

<br>

![srtf](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_PreemptiveSJF_Chart.jpg)


- 평균 대기 시간 = {(10-1) + (1-1) + (17-2) + (5-3)}/4 = 26/4 = 6.5(ms)

=> 비선점 SJF 스케줄링의 경우 평균 대기 시간은 7.75(ms)

<br>

## 우선순위(Priority) 스케줄링

- 우선순위가 가장 높은 프로세스 선택
- 우선순위가 같으면 FCFS
- 선점형 또는 비선점형
- SJF는 우선순위 스케줄링
	- 다음 CPU burst 예상 시간의 역수가 우선순위
- 단점 : **starvation**
	- 낮은 우선순위의 프로세스는 실행되지 않을 수 있다.
	- 해결방법 : **노화(aging)**
		- 일정한 시간마다 프로세스의 우선순위를 증가시킨다.

<br>

| Process | Burst Time | Priority |
| :---: | :---: | :---: |
| P1 | 10 | 3 |
| P2 | 1 | 1 |
| P3 | 2 | 4 |
| P4 | 1 | 5 |
| P5 | 5 | 2 |

<br>

![priority](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_PriorityChart.jpg)

<br>

- 평균 대기 시간 = 8.2(ms)

<br>

## Round Robin(RR) 스케줄링

- 시분할 시스템(time sharing systems)를 위해 설계
- CPU 할당 시간(time quantum _q_)
	- 보통 10\~100ms
	- 시간 경과(인터럽트)되면 프로세스는 선점되고 준비 큐의 마지막에 추가
- time quantum(q)에 따라 성능 결정
	- q가 클수록
		- FCFS에 가까워짐
	- q가 작아직수록
		- 문맥 교환 여러 번 => 오버헤드
		- q는 문맥 교환 시간에 비해 충분히 커야 한다.
			- 보통 문맥 교환 시간은 q의 10%

<br>

![time](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_04_quantums.jpg)

<br><br>

| Process | Burst Time |
| :---: | :---: |
| P1 | 24 |
| P2 | 3 |
| P3 | 3 |

<br>

시간 할당량이 4이고 위와 같이 프로세스가 요청될 때, 간트 차트(Gantt Chart)는 아래와 같다.

<br>

![rr](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_RoundRobinChart.jpg)

<br>

- 평균 대기 시간 = 5.66(ms)
- 일반적으로 SJF보다 총 처리시간은 크지만 **응답시간**은 더 좋음

<br>

## 다단계 큐(Multilevel Queue) 스케줄링

- 프로세스 종류
	- foreground
		- interactive
	- background
		- batch
- ready 큐를 여러 큐로 분류
	- 프로세스는 지정된 큐에 **영구적**으로 할당
	- 각 큐마다 스케줄링 알고리즘 고정
		- foreground : RR
			- 빨리 처리되어야 함
		- background : FCFS
			- 급하지 않음
- 큐와 큐 사이의 스케줄링
	- 고정 우선순위 스케줄링
		- 우선순위가 높은 큐가 모두 비어야 우선순위가 낮은 큐에 있는 프로세스를 선택할 수 있다.
		- **기아** 발생 가능
	- 시간 할당
		- foreground 큐는 80%의 CPU 시간을 할당 받아 Round Robin 스케줄링
		- background 큐는 20%의 CPU 시간을 할당 받아 FCFS 스케줄링

<br>

![queue](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_06_MultilevelQueueScheduling.jpg)

<br><br>

## 다단계 피드백 큐(Multilevel Feedback Queue) 스케줄링

- 앞서 살펴본 다단계 큐 스케줄링에서 프로세스가 큐에 처음 할당되면 영구적으로 그 큐에만 할당됨
- 이는 스케줄링 오버헤드를 줄이지만, 유연하지 못해 기아 발생의 문제가 생길 수 있음
- 다단계 피드백 큐 스케줄링에서는 프로세스가 **큐 간에 이동**할 수 있다.

<br>

- 프로세스의 CPU burst의 길이에 따라 큐 이동
- CPU 시간 할당량 안에 일을 끝내지 못하면 다음 큐로 강등
- 우선순위 낮은 큐에 너무 오래 있던 프로세스는 그 위의 큐로 업그레이드

<br>

![queue](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter6/6_07_MultilevelFeedbackQueues.jpg)

<br>

- 위에서부터 차례대로 Q<sub>0</sub>(RR), Q<sub>1</sub>(RR), Q<sub>2</sub>(FCFS)라 하자
- 새로운 프로세스는 Q<sub>0</sub>으로 진입
- CPU를 할당 받으면 8ms동안 실행
- 8ms 후에 종료되지 않으면, 프로세스는 Q<sub>1</sub>으로 이동
- 16ms 후에 종료되지 않으면, Q<sub>2</sub>로 이동

=> CPU burst가 긴 프로세스는 우선순위가 낮아지는 알고리즘

<br><br>

reference: A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.