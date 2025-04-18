---
title: 소프트웨어 아키텍처 101 - CHAPTER 7 아키텍처 특성 범위
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

아키텍트는 운영 아키텍처 특성을 따져보고 아키텍처 특성에 영향을 미치는 코드베이스 외부의 컴포넌트를 잘 살펴봐야 합니다. 그래서 이런 종류의 의존성을 특정하기 위해 `Building Evolutionary Architectures`(O’reilly, 2017)의 저자들은 아키텍처 퀀텀(architecture quantum)이라는 용어를 정의했습니다. 아키텍처 퀀텀의 의미를 이해하려면 먼저 커네이선스라는 핵심 메트릭을 잘 알아두어야 합니다.

# 7.1 커플링과 커네이선스

<aside>
💡 커네이선스
두 컴포넌트 중 한 쪽이 변경될 경우 다른 쪽도 변경해야 전체 시스템의 정합성이 맞는다면 이들은 커네이선스를 갖고 있는 것이다.

</aside>

아키텍처 퀀텀을 정의하려면 컴포넌트가 어떻게 서로 ‘연결되어(wired)’ 있는지 측정할 방법이 필요한데, 이것이 바로 커네이선스라는 개념입니다.

- 정적 커네이선스
    - 마이크로 서비스 아키텍처의 두 개 이상의 서비스가 동일한 클래스를 공유한다면 해당 서비스들은 서로 정적인 커네이선스를 가진다고 볼 수 있습니다. 다시 말해, 공유 클래스를 변경하면 해당 서비스들도 모두 변경해야 합니다.
- 동적 커네이선스
    - 동적 커네이선스는 동기, 비동기 두 종류가 있습니다.
    - 분산 서비스끼리 호출을 하면 호출부(caller)는 비호출부(callee)의 응답을 기다려야 하는 반면, 이벤트 기반 아키텍처의 비동기 호출은 파이어 앤드 포겟(fire-and-forget) 방식이므로 운영 아키텍처에서 두 서비스는 개별적으로 작동시킬 수 있습니다.

# 7.2 아키텍처 퀀텀과 세분도

소프트웨어를 서로 엮는 것은 컴포넌트 레벨의 커플링만이 아닙니다. 많은 비즈니스 개념이 의미상 여러 시스템 파트를 한데 엮어 기능적으로 응집되어(functional cohesion) 있습니다. 성공적으로 소프트웨어를 설계, 분석, 발전시키기 위해 개발자는 문제가 될 만한 커플링 지점을 모두 살펴봐야 합니다.

<aside>
💡 아키텍처 퀀텀
높은 기능 응집도(high functional cohesion)와 동기적 커네이선스(synchronous connascence)를 가진, 독립적으로 배포 가능한(independently deployable) 아티팩트

- 높은 기능 응집도
응집도는 컴포넌트 설계에 따라 구현된 코드가 얼마나 목적에 맞게 통합되어 있는지를 나타냅니다.
기능 응집도가 높다는 건 아키텍처 퀀텀이 목적에 맞는 뭔가를 하고 있다는 뜻입니다. 단일 데이터베이스를 사용하는 기존 모놀리식 애플리케이션에서는 이런 구분이 별로 의미가 없지만, 마이크로서비스 아키텍처는 보통 개발자가 각 서비스를 하나의 워크플로(’도메인 주도 설계의 경계 콘텍스트’ 코너에서 설명한 경계 콘텍스트)에 맞게 설계하므로 기능 응집도가 높게 나타납니다.

- 동기적 커네이선스
동기적 커네이선스는 아키텍처 퀀텀을 형성하는 애플리케이션 콘텍스트 내부 또는 분산 서비스 간의 동기 호출을 의미합니다.

- 독립적으로 배포 가능
아키텍처 퀀텀은 아키텍처의 다른 파트와 독립적으로 작동되는 모든 필수 컴포넌트를 포함합니다.

</aside>

## 7.2.1 사례 연구: GGG

# 요약

## 7.1 커플링과 커네이선스

- **커네이선스 정의**: 두 컴포넌트 중 하나가 변경되면 다른 쪽도 변경해야 시스템의 정합성이 유지되는 관계.
- **컴포넌트 연결 측정**: 아키텍처 퀀텀을 정의하기 위해 커네이선스를 사용하여 컴포넌트 간의 연결성을 측정함.
- **정적 커네이선스**:
    - 동일한 클래스를 공유하는 두 개 이상의 서비스는 정적 커네이선스를 가짐.
    - 공유 클래스를 변경하면 관련된 모든 서비스도 변경이 필요함.
- **동적 커네이선스**:
    - **동기 방식**: 호출부(caller)가 비호출부(callee)의 응답을 기다림.
    - **비동기 방식**: 파이어 앤드 포겟(fire-and-forget) 방식으로 서비스들이 개별적으로 작동 가능.

## 7.2 아키텍처 퀀텀과 세분도

- **소프트웨어 커플링의 다양성**: 컴포넌트 레벨뿐만 아니라 비즈니스 개념의 기능적 응집도도 고려해야 함.
- **아키텍처 퀀텀 정의**: 높은 기능 응집도와 동기적 커네이선스를 가진, 독립적으로 배포 가능한 아티팩트.
- **높은 기능 응집도**:
    - 컴포넌트가 목적에 맞게 얼마나 통합되어 있는지를 나타냄.
    - 마이크로서비스 아키텍처에서는 각 서비스가 워크플로에 맞게 설계되어 응집도가 높음.
- **동기적 커네이선스**:
    - 애플리케이션 컨텍스트 내부 또는 분산 서비스 간의 동기 호출을 의미함.
- **독립적으로 배포 가능**:
    - 아키텍처의 다른 부분과 독립적으로 작동되는 모든 필수 컴포넌트를 포함함.

## 7.3 사례 연구: GGG

- **사례 연구 소개**: GGG 사례를 통해 아키텍처 퀀텀과 커네이선스의 실제 적용을 분석함.
