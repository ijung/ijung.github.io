---
title: 소프트웨어 아키텍처 101 - CHAPTER 1 서론
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

소프트웨어 아키텍트가 커리어패스가 분명하지 않은 이유

1. 직업 자체에 명확한 정의가 없다.
2. 소프트웨어 아키텍트의 역할은 방대한 분야를 포괄하며, 업무 범위도 계속 넓어지고 있다.
3. 소프트웨어 개발 생태계는 워낙 빠르게 발전하는 분야이고 소프트웨어 아키텍처는 끊임없이 변한다.
4. 소프트웨어 아키택처에 관한 자료는 대부분 역사적인 역사성을 강조한다.

아키텍처를 공부하는 사람들이 명심해야 할 점.

- 아키텍처란 context(문맥, 맥락)로서만 이해할 수 있다.

# 1.1 소프트웨어 아키텍처란?

소프트웨어 아키텍처는 ‘아키텍처 특성(architecture characteristic)’과 ‘아키텍처 결정(architecture decision)’, 그리고 ‘설계 원칙(design principle)’과 시스템의 ‘구조(structure)’로 구성된다.

- 시스템의 구조(structure)
    - 시스템에 구현된 (마이크로서비스, 레이어드, 마이크로커널과 같은) 아키텍처 스타일(들)의 종류
- 아키텍처 특성(architecture characteristic)
    - 소프트웨어 아키텍처를 다른관점에서 바라본 것으로, 일반적으로 시스템의 기능과 직교하는(orthogonal) 시스템의 성공 기준(success criteria)을 결정한다.
- 아키텍처 결정(architecture decision)
    - 시스템 구축에 필요한 규칙들을 정한 것
    - 아키텍처 결정은 시스템의 제약조건(constraint)을 형성하며, 개발자가 해도 되는 것과 하지 말아야 할 것을 알려준다.
- 설계 원칙(design principle)
    - 아키텍처 결정이 반드시 지켜야 할 규칙이라면 설계 원칙은 가이드라인(guideline)이다.
    - 권장하는 방법

# 1.2 아키텍트에 대한 기대치

소프트웨어 아키텍트에게 바라는 핵심 요구사항

- 아키텍처 결정을 내린다.
- 아키텍처를 지속적으로 분석한다.
- 최신 트렌드를 계속 유지한다.
- 아키텍처 결정의 컴플라이언스(compliance)를 보장한다.
- 다양한 기술과 경험에 노출된다.
- 비지니스 도메인 지식을 보유한다.
- 대인 관계 기술이 뛰어나다.
- 정치를 이해하고 처세를 잘한다.

## 1.2.1 아키텍처 결정을 내린다

아키텍트는 아키텍처와 설계 원칙을 결정하고 팀, 부서뿐만 아니라 회사 전체의 기술 결정을 ‘가이드(guide)’하는 사람이다.

## 1.2.2 아키텍처를 지속적으로 분석한다

아키텍트는 끊임없이 아키텍처와 현재 기술 환경을 분석하고 이를 개선하기 위한 해결 방안을 제시한다.

## 1.2.3 최신 트랜드를 계속 따라간다

아키텍트는 항상 최신 기술과 업계 트렌드를 따라가야 한다.

## 1.2.4 아키텍처 결정의 컴플라이언스를 보장한다

아키텍트는 아키텍처 결정과 설계 원칙의 컴플라이언스를 보장해야 한다.

컴플라이언스 보장이란, 아키텍트가 정의하고 문서화하여 전달한 아키텍처 결정과 설계 원칙을을 개발팀이 제대로 준수하고 있는지 지속적으로 확인한다는 뜻.

## 1.2.5 다양한 기술과 경험에 노출된다

아키텍트는 다양한 기술, 프레임워크, 플랫폼, 환경에 노출되어야 한다.

아키텍트가 모든 프레임워크, 플랫폼, 언어에 통달해야 할 필요는 없지만, 적어도 다양한 기술을 꺼리낌 없이 쓸 줄은 알아야 한다.

아키텍트라면 최소한 시스템이나 서비스가 어떤 언어와 플랫폼, 기술로 개발되었는지와 관계없이 다양한 시스템과 서비스를 연동하는 방법을 알고 있어야 한다.

## 1.2.6 비지니스 도메인 지식을 보유한다

아키텍트는 어느 수준 이상의 비지니스 도메인 전문가여야 한다.

## 1.2.7 대인 관계 기술이 뛰어나다

아키텍트는 팀워크, 조정(facilitation), 리더십을 포함한 대인 관계 기술이 뛰어나야 한다.

아키텍트는 개발팀을 기술적으로 이끌기만하는 사람이 아니라, 개발팀을 리드해서 아키텍처를 구현하는 사람이므로 아키텍트라는 직책 또는 역할과 상관없이, 리더십 스킬은 소프트웨어 아키텍트로서 성공하기 위해 필수 요구사항의 최소한 절반 이상은 차지한다.

## 1.2.8 정치를 이해하고 처세를 잘한다

아키텍트는 기업 내부의 정치적 분위기를 이해하고 적절하게 잘 처신할 줄 알아야 한다.

아키텍트는 회사에서 정치를 잘하면서 대부분의 결정을 사람들이 수용하도록 기본적인 협상 기술을 발휘해야 한다.

폭넓고 중요한 결정을 내리는 아키텍트 수준에 이르면 거의 모든 결정을 정당화하고 반대 세력에 맞서 싸울 준비를 갖추어야 한다.

# 1.3 아키텍처의 교차점 그리고…

## 1.3.1 엔지니어링 프랙티스

‘엔지니어링 프랙티스(engineering practice)’와 소프트웨어 개발 ‘프로세스(process)’는 구분해야 한다.

- 프로세스: 팀을 어떻게 구성하고 관리할지, 회의는 어떻게 하고 워크플로 조직은 어떻게 운영할지 등 사람을 조직하고 상호작용하는 총체적인 기법
- 소프트웨어 엔지니어링 프랙티스: 프로세스와 무관하게 가시적으로 반복가능한 혜택을 주는 실천론
- 엔지니어링 프랙티스에 집중하는 것이 중요하다.

## 1.3.2 운영/데브옵스

데브옵스의 출현과 더불어 최근 두드러진 아키텍처와 유관 분야 간의 교차점은 아키텍처 공리를 다시 음미한 결과이다.

마이크로서비스 아키텍처 스타일을 정립한 아키텍트들은 운영 관심사는 운영으로 처리해야 더 매끄럽다는 사실을 깨달았다. 아키텍처와 운영 간의 연결고리를 맺어 설계를 단순화하고, 운영자가 가장 잘 처리할 수 있는 부분은 운영에 맡기게 됐다. 그 과정에서 아키텍트와 운영자는 리소스를 남용하면 뜻밖의 난관에 빠지게 된다는 사실을 깨닫고 서로 의기투합하여 마이크로 서비스를 만들었다.

## 1.3.3 프로세스

소프트웨어 아키텍처는 소프트웨어 개발 프로세스에 거의 직교적이라는 공리가 있다.

<aside>
💡 소프트웨어 아키텍처는 소프트웨어 개발 프로세스에 거의 직교적이라는 공리가 있다.

### “거의 직교적이라는 공리"의 의미

- **직교적(Orthogonal)**: 수학에서 직교라는 개념은 두 직선이 직각으로 교차하는 것을 의미합니다. 이를 비유적으로 사용해서, 두 개념이 서로 독립적이고 상호 간섭이 없다는 의미로 사용됩니다.
- **거의 직교적**: 완전히 독립적이지는 않지만, 대부분의 경우 서로 큰 영향을 미치지 않는다는 의미입니다.

### 구체적 예시

- **아키텍처**: 소프트웨어의 구조를 의미합니다. 예를 들어, 마이크로서비스 아키텍처는 여러 독립적인 서비스로 시스템을 구성하는 방식입니다.
- **프로세스**: 소프트웨어를 개발하는 방법을 의미합니다. 예를 들어, 애자일(Agile) 프로세스는 짧은 개발 주기로 빠르게 제품을 개선하는 방법입니다.

### 예시 상황

1. **애자일 팀 A**: 이 팀은 애자일 방법론을 사용하여 개발합니다. 매일 스탠드업 회의를 하고, 2주마다 스프린트를 마무리합니다. 이 팀은 마이크로서비스 아키텍처를 사용합니다.
2. **워터폴 팀 B**: 이 팀은 워터폴 방법론을 사용합니다. 각 단계(요구 사항 분석, 설계, 구현, 테스트, 유지 보수)를 순차적으로 진행합니다. 이 팀도 마이크로서비스 아키텍처를 사용합니다.
</aside>

소프트웨어를 구축하는 방법(프로세스)은 사실 소프트웨어 아키텍처(구조)에 별다른 영향을 끼치지 않는다.

## 1.3.4 데이터

코드와 데이터는 공생 관계여서 상대방이 없으면 무용지물이다.

데이터베이스 관리자는 아키텍트와 협업하여 복잡한 시스템의 데이터 아키텍처를 구축하며, 관계 및 재사용이 애플리케이션의 포트폴리오에 어떤 영향을 미치는지 분석한다.

# 1.4 소프트웨어 아키텍처 법칙

- 소프트웨어 아키텍처의 모든 것은 다 트레이드오프다.  - 소프트웨어 아키텍처 제1법칙
- 아키텍트가 트레이드오프 아닌 뭔가를 발견했다고 생각했다면 그것은 그가 아직 트레이드오프를 발견하지 못했다는 증거일 가능성이 높다.  - 제1법칙
- ‘어떻게’보다 ‘왜’가 더 중요하다.  - 소프트웨어 아키텍처 제2법칙

# 요약

## 1.1 소프트웨어 아키텍처란?

- 소프트웨어 아키텍처는 아키텍처 특성, 아키텍처 결정, 설계 원칙, 시스템의 구조로 구성됨.
    - **시스템의 구조**: 아키텍처 스타일의 종류 (예: 마이크로서비스, 레이어드, 마이크로커널 등).
    - **아키텍처 특성**: 시스템의 기능과 직교하는 성공 기준을 결정함.
    - **아키텍처 결정**: 시스템 구축에 필요한 규칙과 제약조건을 정함.
    - **설계 원칙**: 아키텍처 결정의 가이드라인을 제시함.

## 1.2 아키텍트에 대한 기대치

- 소프트웨어 아키텍트에게 요구되는 핵심 사항:
    - **아키텍처 결정을 내린다**: 기술 결정을 가이드함.
    - **아키텍처를 지속적으로 분석한다**: 아키텍처와 기술 환경을 개선함.
    - **최신 트렌드를 계속 따라간다**: 최신 기술과 트렌드를 유지함.
    - **아키텍처 결정의 컴플라이언스를 보장한다**: 아키텍처 결정과 설계 원칙의 준수를 확인함.
    - **다양한 기술과 경험에 노출된다**: 다양한 기술을 익히고 적용함.
    - **비지니스 도메인 지식을 보유한다**: 비즈니스 도메인에 대한 지식을 갖춤.
    - **대인 관계 기술이 뛰어나다**: 리더십 및 팀워크 능력을 발휘함.
    - **정치를 이해하고 처세를 잘한다**: 회사 내 정치적 환경에 잘 대처함.

## 1.3 아키텍처의 교차점 그리고…

- **엔지니어링 프랙티스**: 프로세스와 독립적으로 반복 가능한 혜택을 주는 실천론에 집중.
- **운영/데브옵스**: 아키텍처와 운영의 교차점에서 운영이 더 매끄럽게 처리됨.
- **프로세스**: 소프트웨어 개발 프로세스는 아키텍처에 큰 영향을 미치지 않음.
- **데이터**: 코드와 데이터는 공생 관계이며, 아키텍트는 데이터 아키텍처를 구축함.

## 1.4 소프트웨어 아키텍처 법칙

- **제1법칙**: 모든 것은 트레이드오프다.
- **제2법칙**: '어떻게'보다 '왜'가 더 중요하다.
