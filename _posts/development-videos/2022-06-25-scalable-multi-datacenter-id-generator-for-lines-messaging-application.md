---
title: LINE 앱을 위한 확장 가능한 멀티 데이터 센터 ID제너레이터 - 2021 Korean version -
author: June
date: 2022-07-11 23:55:00 +0900
categories: [개발 영상]
tags: [공부, 영상, line, snowflake]
toc: true
math: true
mermaid: true
comments: true
---
## 영상

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/Nj6z8NgKun0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 요약

### 기존 방법

- 레디스를 이용한 ID 생성

### 기존 방법의 문제

- Reliability: 신뢰성
- Availability: 가용성
- Scalability: 확장성

#### Case1: Sudden death of Redis hosts

- Redis Master가 죽을 경우 Redis는 replica와 비동기적으로 동기화를 하므로 ID 중복이 발생할 수 있다.

#### Case2: Network partition

- Master와 Redis Master와 네트워크가 끊길경우 ID 생성이 안 된다.

### 새로운 방법

- Snowflake format 사용: Timestamp + WorkerId + Sequence
- ![snowflake](/posts/development-videos/screenshot-01.png)

### 새로운 방법의 문제

#### Case 1: Not monotonically increasing ID

- 윤초나 NTP에 의한 시간 조정(clock drift)시 ID의 단조로운 증가가 불가능해짐. --> ID 중복 발생
- 해결: clock drift 발생시
  - Keep timestamp monotonic increasing
  - Correct timestamp gradually

#### Case 2: Message lost/dup at Redis storage

- Redis의 lua script의 경우 정수도 부동소수점으로 변환하여 표현하기 때문에 bigint를 올바르게 표현 못 함.(lua script 5.3에서 정수형이 도입되면서 해결)
- lua script에서 bigint를 구현을 하거나, 수치로 변환을 하지 않고 직접 비교 등으로 회피 가능
