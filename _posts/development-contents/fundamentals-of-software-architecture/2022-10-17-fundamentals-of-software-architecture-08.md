---
title: 소프트웨어 아키텍처 101 - CHAPTER 8 컴포넌트 기반 사고
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

아키텍트는 보통 모듈을 물리적으로 구현한 컴포넌트(component)로 생각합니다.

이 장에서는 범위부터 검색에 이르기까지 컴포넌트에 관한 아키텍처 고려 사항을 알아보겠습니다.

# 8.1 컴포넌트 범위

컴포넌트는 아키텍처에서 서브시스템이나 레이어 형태로도 나타나며, 많은 이벤트 프로세서를 위한 배포 가능한 작업 단위입니다.

<aside>
💡 서비스(service)는 또 다른 종류의 컴포넌트로서 자신의 주소 공간에서 실행되며, TCP/IP 같은 저수준 네트워크 프로토콜이나 REST, 메시지 큐 같은 고수준(high-level) 포맷을 통해 통신합니다. 마이크로서비스 같은 아키텍처에서 서비스는 배포 가능한 독립적인 단위를 형성합니다.

</aside>

컴포넌트는 언어가 제공하는 저 수준이 아닌, 더 높은 수준에서 모듈성을 가지는 것이 더 유용할 때가 많습니다.

컴포넌트는 아키텍처의 근본적인 모듈성을 구성하는 요소로서 아키텍트에게 아주 중요한 고려사항입니다. 실제로 아키텍트가 결정하는 중요한 항목 중 하나가 아키텍처 컴포넌트의 최상위 분할과 연관되어 있습니다.

# 8.2 아키텍트 역할

아키텍트는 아키텍처 내부의 컴포넌트를 정의, 개선, 관리, 통제하는 일을 합니다.

새 프로젝트를 시작한 아키텍트는 무엇보다 먼저 컴포넌트를 식별해야 하나, 그 전에 아키텍처를 분할하는 방법을 반드시 이해해야 합니다.

## 8.2.1 아키텍처 분할

컴포넌트는 일반적으로 적재(containership) 메커니즘을 의미하므로 아키텍트는 재량껏 어떤 유형의 분할도 할 수 있습니다.

이 절에서는 최상위 분할(top-level partitioning)이라는 중요한 스타일을 설명하겠습니다.

기술적 최상위 분할(technical top-level partitioning)은 레이어드 아키텍처와 같이 기술적인 능력에 따라 이키텍처를 구성하는 것입니다.

![스크린샷 2024-08-20 오후 6.24.19.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER08/001.png)

<aside>
💡 콘웨이의 법칙 - 시스템을 설계하는 조직은… 그 조직의 소통 구조를 그대로 옮겨 놓은 듯한 설계도를 그릴 수 밖에 없다.

</aside>

도메인 분할은 복잡한 소프트웨어 시스템을 분리하는 모델링 기법을 다룬 에릭 에반스의 `도메인 주도 설계` 에서 비롯됐습니다.

모듈러 모놀리스를 설계하는 아키텍트는 기술적 능력 대신, 도메인이나 워크플로에 따라 아키텍처를 분할합니다. 도메인 분할 아키텍처에서 컴포넌트는 서로 중첩될 때가 많기 때문에 각 컴포넌트는 퍼시스턴스 라이브러리를 사용하거나 별도의 레이어에 비즈니스 규칙을 둘 수 있지만, 어쨌든 최상위 분할은 도메인을 중심으로 전개됩니다.

서로 다른 아키텍처 패턴 간의 근본적인 차이점 중의 하나가 바로 각각의 개별 패턴을 다루는 최상위 분할의 유형입니다.

기술적 분할의 구성 원칙 중 하나는 기술 관심사의 분리(separation of technical concerns)로서, 이는 결국 유용한 수준의 디커플링(decoupling, 반결합, 분리)을 만듭니다.

기술적으로 분할하면 코드베이스가 기능별로 구성되므로 개발자가 코드베이스의 특정 카테고리를 신속하게 찾을 수 있지만, 현실적으로 대부분의 소프트웨어 시스템은 여러 기술/기능을 넘나드는 워크플로를 필요로 합니다.

![스크린샷 2024-08-20 오후 6.38.45.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER08/002.png)

도메인 분할 아키텍처로 설계한 아키텍트는 워크플로 및(또는) 도메인 중심으로 최상위 컴포넌트를 구축합니다. 도메인 분할의 각 컴포넌트는 레이어를 포함한 서브컴포넌트를 가질 수 있지만, 최상위 분할은 도메인에 초점을 두는 까닭에 프로젝트에서 가장 자주 발생하는 변경의 유형들이 더 확실하게 반영됩니다.

## 8.2.2 분할 사례 연구: 실리콘 샌드위치

### 도메인 분할

![스크린샷 2024-08-21 오전 8.12.51.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER08/003.png)

**장점**

- 세부 구현보다 비즈니스 기능에 더 가깝게 모델링 된다.
- 역 콘웨이 전략을 활용하여 도메인 별 다목적팀(cross-functional team)을 구성하기 쉽다.
- 모듈러 모놀리스와 마이크로서비스 아키텍처 스타일에 더 가깝게 맞출 수 있다.
- 메시지 흐름이 문제 영역과 일치한다.
- 데이터와 컴포넌트를 분산 아키텍처로 옮기기 쉽다.

**단점**

- 유저 정의 코드가 여기저기 널려 있다.

### 기술 분할

![스크린샷 2024-08-21 오전 8.14.10.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER08/004.png)

**장점**

- 커스텀 코드가 명확하게 분리된다.
    
    <aside>
    💡 **커스텀 코드**란 특정 애플리케이션 또는 비즈니스 요구사항에 맞추어 특별히 작성된 소프트웨어 코드를 의미합니다.
    
    </aside>
    
- 레이어드 아키텍처 패턴에 더 가깝게 맞출 수 있다.

**단점**

- 전역 커플링(global coupling)이 더 높다. 따라서 공통 또는 로컬 컴포넌트 중 하나라도 변경되면 다른 모든 컴포넌트가 영향을 받을 가능성이 높다.
- 개발자가 공통 레이어, 로컬 레이어 양쪽에 도메인 개념을 복제해야 할 수도 있다.
- 일반적으로 데이터 레벨의 커플링이 높다. 이런 시스템은 대개 애플리케이션 아키텍트, 데이터 아키텍트가 서로 협력하여 단일 데이터베이스를 구성하고 여기에 각종 도메인을 포함시키기 때문에 나중에 아키텍트가 분산 시스템으로 아키텍처를 옮기려고 할 경우 데이터 관계를 파헤치는 작업이 어렵다.

# 8.3 개발자 역할

개발자는 아키텍트와 공동 설계한 컴포넌트를 바탕으로 클래스, 함수, 서브컴포넌트로 더 잘게 나눕니다. 일반적으로 클래스, 함수 설계는 아키텍트, 기술 리더, 개발자의 공동 책임이지만 대부분 개발자가 담당합니다.

개발자는 아키텍트가 설계한 컴포넌트가 최종판이라고 생각해선 안 됩니다. 모든 소프트웨어 설계는 이터레이션을 거쳐 점점 다듬어집니다. 초기 설계는 일단 초안으로 보고 차후 구현을 하며 상세한 것들을 밝히고 하나씩 개선을 하면 됩니다.

# 8.4 컴포넌트 식별 흐름

컴포넌트 식별은 후보를 도출하고 피드백을 통해 다듬어가는 과정을 반복하는 것이 가장 좋습니다.

![스크린샷 2024-08-21 오전 8.48.06.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER08/005.png)

아키텍처는 이런 주기를 반복하면서 점점 구체화됩니다.

## 8.4.1 초기 컴포넌트 식별

아키텍트는 소프트웨어 프로젝트의 소스 코드가 생기기 전에 적용할 최상위 분할의 유형에 따라 최상위 컴포넌트를 어디서부터 시작할지 결정해야 합니다. 초기 식별한 컴포넌트들만으로 제대로 된 설계가 나올 가능성은 거의 없으니 아키텍트는 컴포넌트 설계를 이터레이션하면서 조금씩 개선해야 합니다.

## 8.4.2 요구사항을 컴포넌트에 할당

초기 컴포넌트를 식변한 후, 아키텍트는 컴포넌트에 요구사항(또는 유저 스토리)을 대입해서 잘 맞는지 확인합니다. 이 과정에서 컴포넌트를 새로 만들거나 기존 컴포넌트를 통합하고, 하는 일이 너무 많은 컴포넌트는 분해할 수 있습니다.

## 8.4.3 역할 및 책임 분석

컴포넌트에 스토리를 대입할 때 아키텍트는 요구사항을 파악하는 단계에서 밝혀진 역할과 책임도 살펴보고 세분도(granularity)가 적합한지 확인합니다.

<aside>
💡 세분도 - 세분화한 정도, 즉 얼마나 잘게 나누었는가

</aside>

## 8.4.4 아키텍처 특성 분석

컴포넌트 요구사항을 대입할 때 아키텍트는 앞서 식별한 아키텍처 특성들이 컴포넌트 분할 및 세분도에 어떤 영향을 미치는지 살펴봐야 합니다.

## 8.4.5 컴포넌트 재구성

아키텍트는 개발자들과 함께 지속적으로 컴포넌트 설계를 반복해야 합니다.

# 8.5 컴포넌트 세분도

컴포넌트에서 가장 적당한 세분도를 찾는 것은 아키텍트의 가장 어려운 작업 중 하나입니다. 컴포넌트를 너무 잘게 나누어 설계하면 컴포넌트간 통신이 너무 많아지고, 그렇다고 너무 크게 나누면 내부적으로 커플링이 증가해서 배포, 테스트가 어려워지고 모듈성 관점에서도 부정적인 영향을 미칩니다.

# 8.6 컴포넌트 설계

아키텍트는 아키텍처를 설계하면서 요구사항을 접수하고 애플리케이션을 구성할 굵직굵직한 구성 요소를 그려봐야 합니다. 이 절에서는 컴포넌트를 발견하는 몇 가지 일반적인 방법과 하지 말아야 할 사항을 알려드리겠습니다.

## 8.6.1 컴포넌트 발견

아키텍트는 개발자, 비즈니스 분석가, 도메인 전문가와 협력해서 시스템에 관한 일반적인 지식과 시스템을 어떻게 분할할지(즉, 기술 분할할지 도메인 분할할지) 결정하고 그에 따라 초기 컴포넌트 설계를 합니다.

### 엔티티 함정

엔티티 함정은 아키텍트가 데이터베이스 관계를 애플리케이션의 워크플로로 오해할 때 벌어집니다.(entity trap anti-pattern)

### 액터/액션 접근법

액터/액션 접근법(actor/actions approach)은 아키텍트가 요구사항을 컴포넌트에 매핑할 때 즐겨 쓰는 방법입니다. 아키텍트는 애플리케이션에서 뭔가 일을 하는 액터와 그들이 수행하는 액션을 식별하고 시스템의 대표적인 유저와 이들이 시스템에서 어떤 종류의 일을 하는지 찾아내는 기법입니다.

이 방법은 요구사항 측면에서 역할이 분명하고 그들이 수행하는 액션의 종류가 확실한 경우에 잘 동작하며 아직도 많이 쓰입니다. 이런 방식의 컴포넌트 분해는 모놀리식, 분산 시스템을 비롯한 모든 종류의 시스템에 통용됩니다.

### 이벤트 스토밍

이벤트 스토밍(event storming)은 도메인 주도 설계(DDD)에서 사용되는 컴포넌트 발견 기법입니다. 이벤트 스토밍을 하는 프로젝트에서는 다양한 컴포넌트가 메시지나 이벤트를 이용해 서로 통신한다고 가정합니다. 따라서 팀은 요구사항과 식별된 역할에 따라 시스템에서 어떤 이벤트가 일어나는지 파악하고 컴포넌트를 이벤트와 메시지 핸들러 중심으로 구축합니다. 이 방법은 최종 일관적인(eventual) 시스템에서 사용할 메시지를 아키텍트가 정의하는 데 도움이 되므로 이벤트와 메시지를 사용하는 마이크로서비스 같은 분산 아키텍처에서 주효합니다.

### 워크플로 접근법

워크플로 접근법(workflow approach)은 이벤트 스토밍의 대안으로서, DDD나 메시징을 사용하지 않는, 더 일반화한 방법입니다. 워크플로 기반의 컴포넌트 모델링은 이벤트 스토밍과 비슷하지만 메시지 기반 시스템을 구축하는 데 있어서 명시적인 제약조건은 없습니다. 워크플로 접근법은 핵심 역할을 식별하고 이 역할이 관여하는 워크플로 유형을 결정하며 그렇게 식별된 활동에 따라 컴포넌트를 구축합니다.

# 8.7 컴포넌트 발굴 사례 연구: GGG

# 8.8 아키텍처 퀀텀 딜레마: 모놀로식이냐, 분산 아키텍처냐

시스템이 단일 퀀텀(즉, 한 세트의 아키텍처 특성)만으로 가능하다면 모놀리스 아키텍처가 장점이 더 많습니다. 반면에 컴포넌트마다 아키텍처 특성이 달라지는 경우에는 이를 수용할 수 있는 분산 아키텍처가 필요합니다.

아키텍처 퀀텀을 활용하면 초기 설계 단계에서 아키텍처의 근본적인 설계 특성(모놀리스냐 분산이냐)을 결정할 수 있으므로 아키텍처 특성의 범위와 커플링을 분석하는 방법으로서 장점이 부각됩니다.

# 요약

## 8.1 컴포넌트 범위

- **컴포넌트의 개념**
    - 아키텍트는 컴포넌트를 물리적으로 구현된 모듈로 인식.
    - 서브시스템이나 레이어 형태로 나타나며, 이벤트 프로세서를 위한 배포 가능한 작업 단위로 사용.
- **서비스의 역할**
    - 독립적인 컴포넌트의 한 종류로, 자체 주소 공간에서 실행.
    - TCP/IP, REST, 메시지 큐 등을 통해 통신하며, 마이크로서비스 아키텍처에서 독립적인 배포 단위를 형성.
- **모듈성의 수준**
    - 컴포넌트는 언어의 저수준 모듈성보다 높은 수준에서 유용하게 사용.
    - 아키텍처의 근본적인 모듈성 요소로서 아키텍트에게 중요한 고려사항.
    - 아키텍트는 컴포넌트의 최상위 분할과 관련된 중요한 결정을 내림.

## 8.2 아키텍트 역할

- **컴포넌트 관리**
    - 컴포넌트를 정의, 개선, 관리, 통제.
- **새 프로젝트에서의 역할**
    - 컴포넌트를 식별하기 전에 아키텍처를 분할하는 방법을 이해해야 함.

### 8.2.1 아키텍처 분할

- **컴포넌트 분할의 자유**
    - 컴포넌트는 적재(containership) 메커니즘으로, 아키텍트는 다양한 유형의 분할 가능.
- **최상위 분할 스타일**
    - **기술적 최상위 분할**: 기술적 능력에 따라 아키텍처를 구성 (예: 레이어드 아키텍처).
    - **도메인 분할**: 도메인 주도 설계에서 유래, 도메인이나 워크플로에 따라 아키텍처를 분할.
- **콘웨이의 법칙**
    - 시스템 설계는 그 조직의 소통 구조를 반영하게 됨.
- **도메인 분할의 특징**
    - 컴포넌트 간 중첩이 가능.
    - 퍼시스턴스 라이브러리를 사용하거나 별도의 레이어에 비즈니스 규칙 배치.
    - 최상위 분할은 도메인 중심으로 전개.
- **기술적 분할 vs 도메인 분할**
    - 기술적 분할은 기술 관심사의 분리로 디커플링(decoupling)을 만듦.
    - 현실적인 워크플로는 여러 기술/기능을 넘나듦.

### 8.2.2 분할 사례 연구: 실리콘 샌드위치

### 도메인 분할

- **장점**
    - 비즈니스 기능에 가깝게 모델링.
    - 도메인별 다목적 팀 구성 용이 (역 콘웨이 전략 활용).
    - 모듈러 모놀리스와 마이크로서비스 아키텍처 스타일에 적합.
    - 메시지 흐름이 문제 영역과 일치.
    - 데이터와 컴포넌트의 분산 아키텍처로의 이전 용이.
- **단점**
    - 사용자 정의 코드가 여러 곳에 분산.

### 기술 분할

- **장점**
    - 커스텀 코드의 명확한 분리.
    - 레이어드 아키텍처 패턴에 부합.
- **단점**
    - 전역 커플링이 높아 변경 시 영향 범위가 큼.
    - 개발자가 도메인 개념을 여러 레이어에 복제해야 할 수 있음.
    - 데이터 레벨의 커플링이 높아 분산 시스템으로의 전환이 어려움.

## 8.3 개발자 역할

- **세부 설계**
    - 아키텍트와 공동 설계한 컴포넌트를 바탕으로 클래스, 함수, 서브컴포넌트로 세분화.
- **책임 공유**
    - 클래스, 함수 설계는 아키텍트, 기술 리더, 개발자의 공동 책임이지만 주로 개발자가 담당.
- **설계의 유연성**
    - 컴포넌트 설계는 최종판이 아니며, 구현하면서 상세한 부분을 개선해야 함.

## 8.4 컴포넌트 식별 흐름

- **반복적인 개선**
    - 컴포넌트 식별은 후보를 도출하고 피드백을 통해 다듬어가는 과정을 반복.
- **아키텍처 구체화**
    - 이러한 주기를 통해 아키텍처는 점점 구체화됨.

### 8.4.1 초기 컴포넌트 식별

- **최상위 분할 결정**
    - 아키텍트는 적용할 최상위 분할 유형에 따라 컴포넌트 식별 시작.
- **초기 설계의 중요성**
    - 초기 컴포넌트 설계는 초안으로 보고 이터레이션을 통해 개선.

### 8.4.2 요구사항을 컴포넌트에 할당

- **요구사항 매핑**
    - 식별한 컴포넌트에 요구사항(또는 유저 스토리)을 대입하여 적합성 확인.
- **컴포넌트 조정**
    - 필요에 따라 컴포넌트를 새로 만들거나 통합, 과도한 책임은 분해.

### 8.4.3 역할 및 책임 분석

- **세분도 검토**
    - 요구사항 파악 단계에서 역할과 책임을 검토하여 컴포넌트의 세분도가 적절한지 확인.

### 8.4.4 아키텍처 특성 분석

- **특성 영향 평가**
    - 아키텍처 특성이 컴포넌트 분할 및 세분도에 미치는 영향 분석.

### 8.4.5 컴포넌트 재구성

- **지속적인 개선**
    - 아키텍트는 개발자들과 함께 컴포넌트 설계를 지속적으로 반복하고 개선.

## 8.5 컴포넌트 세분도

- **세분도의 중요성**
    - 컴포넌트의 적절한 세분도를 찾는 것은 아키텍트의 어려운 작업 중 하나.
- **세분화의 균형**
    - 너무 잘게 나누면 통신 오버헤드 증가.
    - 너무 크게 나누면 내부 커플링 증가로 배포와 테스트가 어려워짐.

## 8.6 컴포넌트 설계

- **주요 구성 요소 설계**
    - 아키텍트는 요구사항을 기반으로 애플리케이션의 주요 컴포넌트를 설계.
- **컴포넌트 발견 방법**
    - 여러 가지 일반적인 방법과 주의할 사항을 고려하여 컴포넌트를 발견.

### 8.6.1 컴포넌트 발견

- **협업을 통한 발견**
    - 개발자, 비즈니스 분석가, 도메인 전문가와 협력하여 시스템의 지식과 분할 방법 결정.
- **엔티티 함정 피하기**
    - 데이터베이스 관계를 애플리케이션 워크플로로 오해하는 실수를 주의.
- **액터/액션 접근법**
    - 시스템에서 역할(액터)과 그들이 수행하는 작업(액션)을 식별하여 컴포넌트를 모델링.
    - 요구사항 측면에서 역할이 분명한 경우 효과적.
- **이벤트 스토밍**
    - 도메인 주도 설계(DDD)에서 사용되는 기법으로, 시스템에서 발생하는 이벤트를 기반으로 컴포넌트 구축.
    - 메시지 기반 시스템이나 마이크로서비스 아키텍처에 유용.
- **워크플로 접근법**
    - 이벤트 스토밍의 대안으로 메시징을 사용하지 않는 일반적인 방법.
    - 핵심 역할과 워크플로 유형을 식별하여 컴포넌트를 구축.

## 8.7 컴포넌트 발굴 사례 연구: GGG

- **(해당 내용은 원문에 상세 내용이 없어 생략)**

## 8.8 아키텍처 퀀텀 딜레마: 모놀리식이냐, 분산 아키텍처냐

- **모놀리식 아키텍처의 장점**
    - 단일 퀀텀(한 세트의 아키텍처 특성)으로 가능한 시스템에 적합.
- **분산 아키텍처의 필요성**
    - 컴포넌트마다 아키텍처 특성이 달라지는 경우 분산 아키텍처 필요.
- **아키텍처 퀀텀의 활용**
    - 초기 설계 단계에서 아키텍처의 근본적인 설계 특성(모놀리식 또는 분산)을 결정할 수 있음.
    - 아키텍처 특성의 범위와 커플링을 분석하는 방법으로 장점이 부각됨.
