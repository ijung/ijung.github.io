---
title: 도메인 주도 설계 첫걸음 - 12장 이벤트 스토밍
author: June
date: 2022-09-02 14:43:00 +0900
categories: [개발 책, 도메인 주도 설계 첫걸음(Done)]
tags: [공부, 책, 도메인 주도 설계 첫걸음, 도메인 주도 설계, domain-driven design, DDD, 도메인, 비즈니스, 아키텍처, 소프트웨어 설계]
toc: true
math: true
mermaid: true
comments: true
---
## 범위

- [Part 3] 도메인 주도 설계 적용 실무 - 12장: 이벤트 스토밍

## 개념 정리

- 이벤트 스토밍(eventstorming): 협업을 통해 비즈니스 프로세스를 모델링하는 워크숍
- 자동화 정책(automation policy): 이벤트가 커맨드 실행을 시작하는 시나리오
- 읽기 모델: 도메인에서 액터가 커맨드를 실행하는 의사결정을 내릴 때 사용하는 시각적 데이터

## 책에서 기억하고 싶은 내용

### 이벤트 스토밍이란?

- 사람들이 모여 비즈니스 프로세스에 관해 브레인스토밍하고 신속하게 모델링하기 위한 로우테크(low-tech) 활동
- 비즈니스 도메인 지식을 공유하기 위한 전술적 도구

### 누가 이벤트 스토밍에 참석하나?

- 워크숍에는 다양한 그룹의 사람이 참가하는 것이 제일 좋다.
  - 해결하고자 하는 비즈니스 도메인에 관련된 엔지니어, 도메인 전문가, 제품 소유자, 테스터, UI/UX 디자이너, 지원 담당자 등 누구나 참석할 수 있다.
  - 다양한 배경을 가진 사람이 많이 참여할수록 더 많은 지식이 발견된다.

### 이벤트 스토밍에 무엇이 필요한가?

- 모델링 공간
- 포스트잇
- 마커
- 간식
- 회의실

### 이벤트 스토밍 과정

#### 1단계: 자유로운 탐색(Unstructured Exploration)

- 탐색하려는 비즈니스 도메인에 관련된 도메인 이벤트(domain event)를 브레인스토밍
![그림 12-2](/posts/development-books/learning-domain-driven-design/pic-12-2.png)

#### 2단계: 타임라인(Timelines)

- 생성된 도메인 이벤트를 읽어보고 그것을 비즈니스 도메인에서 발생하는 순서대로 정리
![그림 12-3](/posts/development-books/learning-domain-driven-design/pic-12-3.png)

#### 3단계: 고충점(Pain Points)

- 전체 구성을 보고 프로세스에서 주목할 만한 포인트를 식별
![그림 12-4](/posts/development-books/learning-domain-driven-design/pic-12-4.png)

#### 4단계: 중요 이벤트(Pivotal Events)

- 컨텍스트나 국면이 바뀌는 것을 나타내는 중대한 이벤트를 찾는다.
![그림 12-5](/posts/development-books/learning-domain-driven-design/pic-12-5.png)

#### 5단계: 커맨드(Commands)

- 무엇이 이벤트 또는 이벤트의 흐름을 시작하게 하는지를 설명한다.
![그림 12-6](/posts/development-books/learning-domain-driven-design/pic-12-6.png)

#### 6단계: 정책(Policies)

- 커멘드를 실행할 수 있는 자동화 정책을 찾는다.
![그림 12-7](/posts/development-books/learning-domain-driven-design/pic-12-7.png)

#### 7단계: 읽기 모델(Read Models)

- 액터의 의사결정을 돕는데 필요한 정보의 원천에 대해 짧게 설명한다.
![그림 12-8](/posts/development-books/learning-domain-driven-design/pic-12-8.png)

#### 8단계: 외부 시스템(External Systems)

- 이 단계에서는 외부 시스템 연동 정보를 보강한다.
![그림 12-9](/posts/development-books/learning-domain-driven-design/pic-12-9.png)

#### 9단계: 애그리게이트(Aggregates)

- 커맨드를 받고 이벤트를 생성
![그림 12-10](/posts/development-books/learning-domain-driven-design/pic-12-10.png)

#### 10단계: 바운디드 컨텍스트(Bounded Contexts)

- 서로 연관된 애그리게이트를 찾는다.
![그림 12-11](/posts/development-books/learning-domain-driven-design/pic-12-11.png)

### 변형

- 이벤트 스토밍의 창시자 알베르토 브랜돌리니는 이벤트 스토밍의 진행과정을 반드시 따라야 하는 고정된 규칙이 아니라 가이드라고 했다.
- 즉, 여러분의 상황에 잘 맞는 비법을 찾기 위해 진행 과정에 대해 자유롭게 실험해도 좋다.

### 이벤트 스토밍을 사용하는 경우

- 유비쿼터스 언어 구축하기
- 비즈니스 프로세스 모델링하기
- 새로운 비즈니스 요구사항 탐색하기
- 도메인 지식 복구하기
- 존재하는 비즈니스 프로세스의 개선방법 탐색하기
- 새로운 팀원의 훈련

### 진행 팁

#### 활력도 살피기

- 그룹 활동의 활발함이 떨어지면 질문을 던져서 과정을 북돋거나 워크숍의 다음 단계로 넘어갈지를 결정한다.
- 모든 참가자가 모델링 활동과 논의에 참여할 기회를 갖게 해야 한다.
- 이벤트 스토밍은 집중적인 활동이기 떄문에 휴식이 필요하다.

#### 원격 이벤트 스토밍

- 온라인으로 이벤트 스토밍을 할 때는 커뮤니케이션 효과가 다소 떨어지는 점을 고려하여 좀 더 참을성을 가져야 한다.
- 원격 이벤트 스토밍 세션은 참가자의 수가 적을 때 더욱 효과적이다.
- 여건이 허락된다면 다시 대명 이벤트 스토밍으로 돌아간다.

### 결론

- 이벤트 스토밍은 자전거를 다는 것과 같다.
  - 책을 읽는 것보다 타면서 배우는 것이 훨씬 쉽다.
