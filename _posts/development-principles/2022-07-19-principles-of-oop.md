---
title: 객체 지향 프로그래밍의 5원칙 - S.O.L.I.D
author: June
date: 2022-07-19 01:00:00 +0900
categories: [개발 원칙]
tags: [개발 원칙, oop, 객체지향 프로그래밍]
toc: true
math: true
mermaid: true
comments: true
---
## 객체 지향 프로그래밍이란?

- 객체 지향 프로그래밍(영어: Object-Oriented Programming, OOP)은 컴퓨터 프로그래밍의 패러다임 중 하나이다. 객체 지향 프로그래밍은 컴퓨터 프로그램을 명령어의 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.[[객체 지향 프로그래밍](https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)]

## 객체 지향 프로그래밍의 5원칙(SOLID)이란?

- 객체지향에서 꼭 지켜야 할 5개의 원칙을 통틀어 객체지향 5원칙이라 칭한다. 일단 한번 보면 개념은 알아 듣긴 하지만 막상 실현하려면 생각보다 어려움이 따른다. 이 5개의 원칙의 앞글자를 따서 SOLID라고도 부른다.[[객체 지향 프로그래밍/원칙](https://namu.wiki/w/%EA%B0%9D%EC%B2%B4%20%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%EC%9B%90%EC%B9%99)]
- 객체 지향 프로그래밍의 시작과 끝은 응집도, 결합도, 의존성으로 정리되며, 이 원칙들은 이 것을 위해 존재한다.

## 객체 지향 프로그래밍의 5원칙 - S.O.L.I.D

1. Single Responsibliity Principle(SRP, 단일 책임 원칙)
    - 객체는 오직 하나의 책임(변경의 이유)을 가져야 한다.
2. Open-Closed Principle(OCP, 개방-폐쇄 원칙)
    - 객체는 확장에 대해서는 개방적이고, 수정에 대해서는 폐쇄적이어야 한다.
3. Liskov Substitution Principle(LSP, 리스코프 치환 원칙)
    - 자식 클래스는 언제나 자신의 부모 클래스를 대체할 수 있다. 즉, 부모 클래스가 들어갈 자리에 자식 클래스를 넣어도 계획대로 잘 작동해야 한다.
    - 상속의 본질
    - 이를 지키지 않으면 부모 클래스 본래의 의미가 변해서 is-a 관계가 망가져 다형성을 지킬 수 없게 된다.
4. Interface Segregation Peinciple(ISP, 인터페이스 분리 원칙)
    - 클라이언트에서 사용하지 않는 메서드는 사용해선 안 된다. 그러므로 인터페이스를 다시 작게 나누어 만든다.
5. Dependency Inversion Principle(DIP, 의존성 역전 원칙)
    - 추상성이 높고 안정적인 고수준의 클래스는 구체적이고 불안정한 저수준의 클래스에 의존해서는 안 된다.

## Reference

- [객체 지향 프로그래밍](https://ko.wikipedia.org/wiki/%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [객체 지향 프로그래밍/원칙](https://namu.wiki/w/%EA%B0%9D%EC%B2%B4%20%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D/%EC%9B%90%EC%B9%99)