---
title: 헥사고날 아키텍처
author: June
date: 2022-08-22 02:19:00 +0900
categories: [개발 아키텍처, 헥사고날 아키텍처]
tags: [아키텍처, 헥사고날 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 헥사고날 아키텍처(Hexagonal architecture)란?

- 소프트웨어 설계에 사용되는 아키텍처 패턴 중 하나
- 헥사고날 아키텍처(Hexagonal architecture) 또는 포트 및 어댑터 아키텍처(ports and adapters architecture)라고 불립니다.
- '헥사고날(육각형)'이라는 용어는 육각형 셀과 같은 응용 프로그램 구성 요소를 보여주는 그래픽 규칙에서 비롯됩니다. 그 목적은 여섯 개의 경계 / 포트가있을 것이라고 제안하는 것이 아니라 구성 요소와 외부 세계 사이에 필요한 다양한 인터페이스를 나타낼 수있는 충분한 공간을 남겨 두는 것이 었습니다.

![Hexagonal architecture](/posts/development-architectures/hexagonal-architecture.svg)

## 목표

- 포트와 어댑터를 통해 소프트웨어 환경에 쉽게 연결할 수 있는 느슨하게 결합된(Loose coupling) 응용 프로그램 구성 요소를 만드는 것을 목표로 합니다.
- 이를 통해 모든 수준에서 구성 요소를 교환할 수 있으며 테스트 자동화가 용이하게 됩니다.

## 원칙

- 각 구성 요소는 다수의 노출된 '포트'를 통해 다른 구성 요소에 연결됩니다.
  - 이러한 포트를 통한 통신은 목적에 따라 주어진 프로토콜을 따릅니다. 포트와 프로토콜은 적절한 기술 수단(예: 객체 지향 언어의 메서드 호출, 원격 프로시저 호출 또는 웹 서비스)으로 구현할 수 있는 추상 API를 정의합니다.
- '어댑터'는 구성 요소와 외부 세계를 연결하는 접착제입니다.
  - 외부 세계와 애플리케이션 구성 요소 내부의 요구 사항을 나타내는 포트 간의 교환을 조정합니다.
  - 하나의 포트에 여러 어댑터가 있을 수 있습니다.

## 헥사고날 아키텍처(Hexagonal architecture)와 레어어드 아키텍처(Layered architecture)의 차이

### 의존성 방향

- 헥사고날 아키텍처의 의존성은 안으로 향한다.
  - 즉, Adapter의 변화가 생기더라도 Application은 유지된다.
- 레어어드 아키텍처의 의존성은 아래로 향한다.
  - 즉, Persistence Layer의 변화가 생기면 Business도 변경이 필요하다.

![헥사고날 아키텍쳐와 레어어드 아키텍쳐의 의존성 방향 차이](/posts/development-architectures/hexagonal-architecture_vs_layered-architecture.jpg)

### 복잡도

- 헥사고날 아키텍처는 작은 서비스에서도 port 등 신경써야 할 사항이 상대적으로 많다.
- 레어어드 아키텍처는 상대적으로 단순하다.

### 테스트 코드 작성

- 헥사고날 아키텍처는 port에 mock을 주입하는 방식으로 손쉽게 단위 테스트 코드 작성이 가능한다.
- 레어어드 아키텍처는 비지니스가 Persistence Layer에 대해 의존성이 있어 단위 테스트 작성에 신경써야 할 사항이 상대적으로 많다.

## Reference

- [Hexagonal architecture (software)](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))

## Attachments

- [이미지 원본](https://1drv.ms/p/s!AvoR1zNfvX11kbZXaVtoMUPX1L21wA?e=FsuWdC)
