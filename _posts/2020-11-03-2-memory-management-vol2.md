---
layout : post
title : "메모리 관리(2) : 메모리 할당"
author : "DAEUN"
---

### 고정분할(multiple-partition)

- 모든 프로세스에게 동일한 크기의 메모리 할당
- 분할의 개수가 다중 프로그래밍의 정도를 결정

<br>

### 가변분할(variable-partition)

- 프로세스의 요청에 맞춰 메모리 할당
- OS는 메모리 상태를 파악할 수 있는 테이블을 갖고 있음
- **Hole** : 새로운 프로세스가 할당될 수 있는 메모리 내의 공간


- 동적 메모리 할당 문제
	- 프로세스의 크기를 만족시키는 hole을 어떻게 찾나?
		- 최초 적합(first-fit)
			- 충분히 큰 최초의 hole 할당
			- 시작 위치는 맨 처음의 hole일수도, 이전에 first-fit이 끝난 지점일수도
		- 최적 적합(best-fit)
			- 충분히 큰 공간 중에서 제일 작은 hole 할당
			- 크기순으로 정렬되어있지 않다면, 전체 hole 집합을 탐색해야 함
		- 최악 적합(worst-fit)
			- 제일 큰 hole 할당
			- 크기순으로 정렬되어있지 않다면, 전체 hole 집합을 탐색해야 함
	- 모의 실험 결과
		- **최초 적합**과 **최적 적합**이 최악 적합보다 속도 및 공간 이용률 측면에서 낫다.
		- 시간 측면에서는 최초 적합이 가장 빠름

<br><br>

refereces : A. Silberschatz, P.B. Galvin, and G. Gagne, _Operating System Concepts Essentials_, 2nd ed. Hoboken, NJ, USA: Wiley, 2014.