---
layout: post
title: "Process Scheduling"
author: "DAEUN"
---

## 멀티 프로그래밍(Multiprogramming)

- 목적: CPU를 최대한 효율적으로 사용하기 위함
- 개념: 단일 프로세서 상에서 여러 개의 프로세스를 병행 실행 -> 동시에 실행되는 것처럼 보임
	- 프로세스의 입출력은 CPU가 관여하지 않음 -> 입출력이 끝날 때까지 기다린다면 CPU는 idle 상태로 자원 낭비 -> 입출력을 기다리지 않고 **process scheduler**가 다른 프로세스를 선택해 CPU 할당

<br>

## 프로세스 스케줄링

- 목적 : CPU 이용의 극대화
- job queue
	- 현재 시스템에 있는 모든 프로세스의 집합
- **ready queue**
	- ready 상태에서 실행을 기다리는 프로세스 집합
	- 연결 리스트로 구현
	- 헤더는 첫 번째와 마지막 PCB를 가리키는 포인터 존재
	- 각 PCB는 다음 PCB를 가리키는 포인터 존재
- **device queue**
	- I/O 장치를 기다리는 프로세스 집합

<br>

## 스케줄러

### 장기 스케줄러(long-term/job scheduler)

너무 많은 프로세스를 실행하려는 경우, 모두 메모리에 적재하지 않고 디스크에 임시로 저장하고 실행을 기다린다. 이 때 장기 스케줄러가 선택한 프로세스에 메모리를 할당해 ready queue로 이동하는 역할을 한다.

- job queue -> ready queue
- 프로세스 상태 : new -> ready
- 멀티프로그래밍의 정도를 제어
	- 메모리에 있는 프로세스의 개수를 제어한다.
- I/O-bound process와 CPU-bound process를 적절히 선택하는 것이 중요
	- **I/O-bound process** : 입출력에 더 많은 시간이 걸리는 프로세스. running < **waiting**
	- **CPU-bound process** : 계산에 더 많은 시간이 걸리는 프로세스. **running** > waiting
	- I/O-bound process가 너무 많으면
		- reqdy 큐는 비어 있고 CPU 자원 낭비
	- CPU-bound process가 너무 많으면
		- device 큐는 비어 있고 장치 사용하지 않음

<br>

### 단기 스케줄러(short-term/CPU scheduler)

ready 큐에 있는 프로세스를 선택해 CPU를 할당한다.

- 프로세스 상태 : ready -> running
- 시분할 시스템(time-sharing system. 예) UNIX, Windows)에서는 장기 스케줄러 없이 단기 스케줄러만 존재하기도 함
	- 새로 실행된 프로세스는 디스크가 아닌 메모리에 적재해 단기 스케줄러가 관리

<br>

### 장기 vs 단기 스케줄러

|	| 장기 스케줄러 | 단기 스케줄러 |
| --- | --- | --- |
| 실행 빈도 | 드물다 (n min마다 실행) | 잦다 (n ms마다 실행) |
| 프로세스 선택 시간 | 길어도 괜찮다 | 빨라야한다 |

<br>

### 중기 스케줄러(midium-term scheduler)

![scheduler](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter3/3_07_QueuingDiagram2.jpg)

<br>

- 목적 : 다중프로그래밍의 정도를 낯춰 메모리 공간을 여유롭게 한다.
- 언제 : I/O와 CPU-bound 프로세스의 밸런스를 맞춰야하거나 메모리 공간이 부족할 때
- 기술 : **swapping**
	- swap out : 메모리 -> 디스크
	- swap in : 디스크 -> 메모리