---
title: MSA(microservice architecture, 마이크로서비스 아키텍처)의 특징
author: June
date: 2022-07-17 14:08:00 +0900
categories: [개발 아키텍처, 마이크로 서비스 아키텍처]
tags: [아키텍처, msa, microservice architecture, 마이크로서비스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 정의

- 마이크로서비스(microservice)는 애플리케이션을 느슨하게 결합된 서비스의 모임으로 구조화하는 서비스 지향 아키텍처(SOA) 스타일의 일종인 소프트웨어 개발 기법이다.[[마이크로서비스](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4)]
- “단일 애플리케이션을 작은 서비스 모음으로 개발하는 접근 방식으로서 작은 서비스는 자체적으로 실행할 수 있고, 경량 메커니즘을 통해 서로 통신한다. 이런 서비스는 비즈니스 기능을 기반으로 구축되며, 독립적으로 자동화하여 배포될 수 있다. 서로 다른 프로그래밍 언어로 작성되고 다른 데이터 저장 기술을 사용할 수 있기 때문에 이 서비스의 중앙 집중식 관리는 거의 필요하지 않다[Martin Fowler 2014].”

## 특징

1. Componentization via Services
: 서비스의 컴포넌트화
2. Organized around Business Capabilities
:비지니스 중심으로 구성
3. Products not Projects
:프로젝트가 아닌 제품
4. Smart endpoints and dumb pipes
: 스마트 엔트포인트와 덤 파이프
5. Decentralized Governance
: 개발 분권화
6. Decentralized Data Management
: 분산 데이터 관리
7. Infrastructure Automation
: 인프라 자동화
8. Design for failure
: 장애를 전제로 한 설계
9. Evolutionary Design
: 진화하는 설계

## Reference

- [마이크로서비스](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4)
- [Microservices](https://martinfowler.com/articles/microservices.html)
