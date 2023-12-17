---
title: JAVA의 동시성 프로그래밍
author: June
date: 2022-08-23 10:47:00 +0900
categories: [개발 언어, JAVA]
tags: [개발 언어, JAVA, 동시성 프로그래밍]
toc: true
math: true
mermaid: true
comments: true
---
## 스레드(Thread)

### 스레드(Thread)란?

- 스레드(thread)는 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다.
- 일반적으로 한 프로그램은 하나의 스레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다.

### 스레드(Thread)와 프로세스(Process)의 차이

- 프로세스(process)는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램을 말한다.
- 멀티 프로세스에서 각 프로세스는 독립적으로 실행되며 각각 별개의 메모리를 차지하고 있는 것과 달리 멀티 스레드는 프로세스 내의 메모리를 공유해 사용할 수 있다.
- 스레드간 context swiching 오버헤드가 프로세스간 conext swiching 보다 더 작다.

![각각 두 개의 스레드를 실행하고 있는 두 개의 프로세스](/posts/development-languages/java/multithreaded-process.jpg)

### JAVA에서 Thread 사용 방법

- Runnable를 구현한다.
- Callable(JAVA 1.5 이상)을 구현한다.
- Runnable과 Callable의 차이
  - 내부동작은 거의 동일 하다.
  - Runnable은 run 메소드에 task를 정의하고, Callable은 call 메소드로 task를 정의한다.
  - run은 void 리턴이며, call은 리턴값을 가진다.
  - call은 예외 처리가 가능하다.

### Thread Pool

- 스레드들이 존재할 수 있는 영역
- 프로그램이 task를 생성하면 task가 Queue에 들어가고, Queue를 통해 Thread Pool에 있는 Thread 중 작업할당이 되지 않은 Thread에게 작업을 할당한다.
- Thread가 task를 끝냈다면, 그 Thread는 없어지지 않고, 다시 일 할 수 있는 상태로 돌아가서 Queue에서 넘어오는 Task를 할당받길 기다린다.

## 동시성 제어

### 멀티 스레드에서 동시성 제어가 필요한 이유

- 멀티 스레드 환경에서는 공유 자원에 대한 경합(race condition)이 발생할 수 있다.
- 경합이 발생할 경우 원하는 결과 값이 나오지 않거나 교착 상태 등으로 프로그램이 원하지 않는 동작을 할 수 있다.
- 그렇게 때문에 동시성 제어가 필요하다.

### JAVA에서의 동시성 제어 솔루션

#### volatile

- JAVA 변수를 CPU의 Cache영역이 아닌 Main Memory에 저장하겠다라는 것을 명시하는 키워드
- 멀티 스레드 환경에서 스레드가 변수 값을 읽어올 때 각각의 CPU Cache에 저장된 값이 다르기 때문에 변수 값 불일치 문제가 발생할 수 있다.
- 멀티 스레드 환경에서 하나의 스레드만 write를 수행하고, 나머지 스레드는 read를 한다면 언제나 최신 값을 얻어 올 수 있게된다.
- 원자성을 보장하는 키워드는 아니기 때문에 여러 스레드가 write를 하는 환경에서는 사용하지 않는다.

#### synchronized

- 멀티 스레드 환경에서 동시성제어를 위해 공유객체를 동기화 하는 키워드
- synchronized 블록 안에서 관리되는 자원들은 원자성을 보장할 수 있다.
  - synchronized 키워드로 scope을 지정해놓고 멀티쓰레드가 동시에 접근하지 못하도록 blocking하는 것

#### atomic

- 멀티 스레드 환경에서 원자성을 보장하는 키워드
- blocking이 아닌 CAS(Compared And Swap) 알고리즘으로 작동하여 원자성을 보장한다.
- CAS(Compared And Swap) 알고리즘은 값 변화가 필요한 시점에 값을 가져와 변화를 시킬 때 현재 값이 가져온 값과 같으면 변경을 하고 다르면 재시도를 하는 알고리즘이다.

## Reference

- [스레드_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85))
- [프로세스](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)

## Attachments

- [이미지 원본](https://1drv.ms/p/s!AvoR1zNfvX11kbZYu80Blu2fJz6MyQ?e=DqYOZ5)
