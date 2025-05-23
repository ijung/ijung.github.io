---
title: 소프트웨어 아키텍처 101 - CHAPTER 12 마이크로 아키텍처 스타일
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

마아크로커널 아키텍처(microkernel architecture, 플러그인 아키텍처(plug-in architecture)라고도 함) 스타일은 (단일 모놀리식 배포 단위로 패키징해서 다운로드 및 설치가 가능하며, 보통 고객 사이트에서 서드파티 제품으로 설치되는) 제품 기반(product-based) 애플리케이션에 적합하며, 비제품(nonproduct) 고객 비즈니스 애플리케이션에서도 많이 사용됩니다.

# 12.1 토플로지

마이크로커널 아키텍처 스타일은 코어 시스템(core system)과 플러그인 컴포넌트(plug-in component)라는 두 가지 아키텍처 요소로 구성된 비교적 단순한 모놀리식 아키텍처입니다.

![스크린샷 2024-09-10 오전 9.04.37.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER12/001.png)

## 12.1.1 코어 시스템

코어 시스템은 시스템을 실행시키는 데 필요한 최소한의 기능으로 정의합니다. 코어 시스템은 커스텀 처리가 거의/전형 필요 없는, 애플리케이션을 관통하는 정상 경로(happy path, 일반적인 처리 흐름)라고 정의할 수 있습니다. 코어 시스템의 순환 복잡도를 없애고 별도의 플러그인 컴포넌트를 장착하면 확장성, 유지보수성은 물론 시험성도 좋아집니다.

코어 시스템은 규모와 복잡도에 따라 레이어드 아키텍처나 모듈러 모놀리스로 구현할 수 있습니다[그림 12-2]. 경우에 따라 코어 시스템을 별도 배포하는 도메인 서비스로 나누어 서비스별 도메인에 특정한 플러그인 컴포넌트를 둘 수도 있습니다.

![스크린샷 2024-09-10 오전 9.19.51.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER12/002.png)

코어 시스템의 프레젠테이션 레이어는 코어 시스템에 내장하거나 별도의 UI로 구현하고 코어 시스템은 백엔드 서비스를 제공합니다.

![스크린샷 2024-09-10 오전 9.21.16.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER12/003.png)

실제로 별도의 UI를 마이크로 커널 아키텍처 스타일로 구현할 수도 있습니다. [그림 12-3]은 코어 시스템에 대해 프레젠테이션 레이어를 구성하는 세 가지 변형입니다.

## 12.1.2 플러그인 컴포넌트

플러그인 컴포넌트는 특수한 처리 로직, 부가 기능, 그리고 코어 시스템을 개선/확장하기 위한 커스텀 코드가 구형된 스탠드얼론 컴포넌트입니다. 변동성이 매우 큰 코드를 분리하여 애플리케이션 내부의 유지보수성, 시험성을 높이는 것입니다. 이상적인 플러그인 컴포넌트는 상호 독립적이며 의존성이 없습니다.

플러그인 컴포넌트와 코어 시스템은 일반적으로 점대점(point-to-point) 통신을 합니다. 즉, 코어 시스템에 플러그인을 연결하는 ‘파이프’는 대부분 플러그인 컴포넌트의 진입점 클래스(entry-point class)를 호출하는 메서드나 함수 코드입니다.

플러그인 컴포넌트가 중앙 공유 데이터베이스에 직접 접속할 일은 거의 없습니다. 외려 코어 시스템이 그 역할을 담당하며 필요한 데이터를 모두 가져와 각 플러그인에게 전달합니다. 이렇게 하는 가장 큰 이유는 디커플링입니다.

# 12.2 레지스트리

코어 시스템은 어떤 플러그인을 사용할 수 있는지, 그 플러그인을 가져오려면 어떻게 해야 하는지 알고 있어야 합니다. 가장 일반적인 구현 방법은 플러그인 레지스트리(registry)를 경유하는 것입니다. 이 레지스트리에는 플러그인 명칭, 데이터 계약, (플러그인에서 코어 시스템으로 접속하는 방법별) 세부 원격 액세스 프로토콜 등 각 플러그인 모듈에 관한 정보가 있습니다.

# 12.3 계약

플러그인 컴포넌트와 코어 시스템 간의 계약은 보통 플러그인 컴포넌트의 모데인 단위로 표준화되어 있고, 플러그인 컴포넌트가 수행하는 기능 및 입출력 데이터는 계약에 명시되어 있습니다.

# 12.4 실제 용례

# 12.5 아키텍처 특성 등급

![스크린샷 2024-09-11 오전 8.11.23.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER12/004.png)

마이크로커널 아키텍처도 레이어드 아키텍처처럼 단순성과 전체 비용은 주요 갈점입니다. 반면, 고질적인 모놀리식 배포 탓에 탄력성, 내고장성, 확장성이 문제가 될 때가 많습니다. 모든 요청은 코어 시스템을 통해 유입되어 독리적인 플러그인 컴포넌트로 흘러가므로 퀀텀은 언제나 1입니다.

# 요약

## 12.1 토폴로지 (Topology)

- **마이크로커널 아키텍처**: 코어 시스템과 플러그인 컴포넌트로 구성된 단순한 모놀리식 아키텍처.
- **코어 시스템**: 시스템 실행에 필수적인 최소한의 기능을 제공하며, 확장성과 유지보수성을 높임.
- **플러그인 컴포넌트**: 특수한 로직과 부가 기능을 제공하며, 코어 시스템을 확장하고 개선하는 역할.

### 12.1.1 코어 시스템 (Core System)

- **핵심 기능**: 커스텀 처리가 거의 없는 정상 경로 제공.
- **구현 방식**: 레이어드 아키텍처나 모듈러 모놀리스로 구현 가능.
- **프레젠테이션 레이어**: 코어 시스템에 통합하거나 별도의 UI로 제공.

### 12.1.2 플러그인 컴포넌트 (Plug-in Component)

- **역할**: 특수 로직, 부가 기능, 커스텀 코드 제공.
- **구성**: 상호 독립적이며, 코어 시스템과 점대점 통신.
- **데이터 접근**: 플러그인은 데이터베이스에 직접 접근하지 않고 코어 시스템을 통해 데이터를 전달받음.

## 12.2 레지스트리 (Registry)

- **역할**: 코어 시스템이 사용할 수 있는 플러그인을 관리.
- **구성 요소**: 플러그인 명칭, 데이터 계약, 원격 액세스 프로토콜 등의 정보 포함.

## 12.3 계약 (Contract)

- **계약 내용**: 플러그인과 코어 시스템 간의 표준화된 계약을 통해 입출력 데이터와 기능을 명시.

## 12.4 실제 용례 (Real-world Use Cases)

- 마이크로커널 아키텍처의 실제 적용 사례 설명.

## 12.5 아키텍처 특성 등급 (Architectural Characteristics Ratings)

- **장점**: 단순성과 전체 비용이 낮음.
- **단점**: 모놀리식 배포로 인해 탄력성, 내고장성, 확장성에 문제 발생 가능.
- **작동 방식**: 모든 요청이 코어 시스템을 통해 플러그인으로 전달됨.
