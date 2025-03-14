---
title: 소프트웨어 아키텍처 101 - CHAPTER 5 아키텍처 특성 식별
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

아키텍처를 구축하거나 기존 아키텍처의 타당성을 검증할 때 제일 먼저 해야 할 일은 아키텍처 특성을 식별하는 것입니다. 아키텍트는 해당 도메인을 잘 이해하고 있어야 하며, 도메인 관점에서 정말 중요한 것들을 결정해야 합니다.

# 5.1 도메인 관심사에서 아키텍처 특성 도출

아키텍트는 도메인 관심사를 올바르게 해석하여 정확한 아키텍처 특성을 식별해야 합니다.

- 아키텍처 특성을 정의하는 한 가지 팁.
    
    **최종 목록을 가능한 한 짧게 하라.**
    지원 가능한 아키텍처 특성 하나하나가 전체 시스템 설계를 복잡하게 만드는 요인이기 때문에 너무 많은 아키텍처 특성을 수용하면 아키텍처가 너무 복잡해져 버립니다. 아키텍처 특성의 개수에 연연하지 말고 가급적 설계를 단순화하는 게 좋습니다.
    

아키텍트와 도메인 이해관계자들은 서로 다른 언어로 말을 합니다. 도메인 관심사를 아키텍처 특성으로 옮겨야 하는데, [표 5-1]은 일반적인 도메인 관심사와 이를 뒷받침하는 아키텍처 특성을 정리한 표입니다.

표 5-1 도메인 관심사를 아키텍처 특성으로 옮긴다.

| 도메인 관심사 | 아키텍처 특성 |
| --- | --- |
| 인수 합병 | 상호운용성, 확장성, 적응성, 신장성 |
| 출시 시기 | 민첩성, 시험성, 배포성 |
| 유저 만족 | 성능, 가용성, 내고장성, 시험성, 배포성, 민첩성, 보안 |
| 경쟁 우위 | 민첩성, 시험성, 배포성, 확장성, 가용성, 내고장성 |
| 시간 및 예산 | 단순성, 실행성 |

# 5.2 요구사항에서 아키텍처 특성 도출

요구사항 정의서에 명시된 문장에서 도출한 아키텍처 특성도 있습니다. 아키텍트가 알고 있는 도메인 지식에서 도출되는 특성들도 있는데, 이것이 아키텍트가 도메인 지식을 갖고 있으면 이로운 이유입니다. 아키텍트는 요구사항 정의서에 적혀 있지 않은 시시콜콜한 내용까지 어떤 식으로든 설계 결정에 반영해야 합니다.

# 5.3 사례 연구: 실리콘 샌드위치

- 시나리오
    
    ![스크린샷 2024-07-29 오후 12.38.02.png](/posts/development-books/fundamentals-of-software-architecture/CHAPTER05/001.png)
    

## 5.3.1 명시적 특성

명시적 아키텍처 특성은 필요한 설계의 일부로서 요구사항 정의서에 기술됩니다. 그리고 도메인 레벨에서의 기대치를 고려해야 합니다.

시나리오에서 눈여겨 봐야 할 부분

- 유저 수
    - 지금은 수천 명 정도에 불과하나 향후 수백만 명에 이를 수 있다.
    - 도출되는 아키텍처 특성: 확장성 - 유의미한 성능 저하 없이 다수의 동시 유저를 처리하는 능력
- 도메인 특징
    - 트래픽이 식사시간 전후로 급증한다.
    - 잠재된 아키텍처 특성: 탄력성 - 순간적으로 폭증한 유저 요청을 처리하는 능력

## 5.3.2 암묵적 특성

- 요구사항 정의에 따로 없는 암묵적인 아키텍처 특성
    - ex) 가용성(availability), 신뢰성(reliability) 등
- 아키텍처 특성은 서로 연관되어 움직이므로 중요도(criticality)에 따라 우선 순위는 달라질 수 있습니다.

<aside>
💡 아키텍처에서는 틀린 답은 없고 값비싼 답만 있다.

</aside>

- 개발자는 어떤 방법을 쓰더라도 기능을 구현할 수 있으므로 아키텍트는 아키텍처 특성을 정확하게 발견하려고 너무 스트레스를 받을 필요는 없습니다.

<aside>
💡 아키텍처에서 최고의 설계는 없다. 오직 나쁜 것 중에서 제일 나은 트레이드오프들만 있을 뿐!

</aside>

- 아키텍트는 아키텍처 특성에 우선 순위를 매겨 가장 단순한 필수 세트로 정리해야 합니다.
    - 팀원들이 1차적으로 아키텍처 특성을 식별하고 나서 가장 덜 중요한 것을 솎아내는 훈련도 도움이 됩니다.

# 요약

## 5.1 도메인 관심사에서 아키텍처 특성 도출

- **아키텍트의 역할**
    - 도메인 관심사를 올바르게 해석하여 정확한 아키텍처 특성을 식별해야 함.
- **아키텍처 특성 정의 팁**
    - 최종 목록을 가능한 한 **짧게** 유지.
    - 너무 많은 특성은 시스템 설계를 복잡하게 만듦.
    - 특성의 개수에 연연하지 말고 설계를 **단순화**하는 것이 중요함.
- **도메인 관심사와 아키텍처 특성 변환**
    - 아키텍트와 이해관계자는 서로 다른 언어를 사용하므로 변환 필요.
    - **표 5-1**: 일반적인 도메인 관심사와 대응하는 아키텍처 특성 정리.

## 5.2 요구사항에서 아키텍처 특성 도출

- **요구사항 정의서로부터의 도출**
    - 명시된 문장에서 아키텍처 특성을 추출할 수 있음.
- **아키텍트의 도메인 지식 활용**
    - 도메인 지식에서 특성을 도출하는 것이 이점이 됨.
    - 요구사항에 적혀 있지 않은 **사소한 내용까지** 설계에 반영해야 함.

## 5.3 사례 연구: 실리콘 샌드위치

### 5.3.1 명시적 특성

- **유저 수 증가 전망**
    - 현재 수천 명에서 **향후 수백만 명**으로 증가 가능성.
    - **도출된 특성**: **확장성**
        - 성능 저하 없이 다수의 동시 유저를 처리하는 능력.
- **도메인 특징**
    - 식사 시간 전후로 트래픽 급증 현상.
    - **잠재된 특성**: **탄력성**
        - 순간적인 유저 요청 폭증을 처리하는 능력.

### 5.3.2 암묵적 특성

- **명시되지 않은 특성의 존재**
    - 요구사항에 없지만 중요한 특성들 (예: 가용성, 신뢰성).
- **특성 간의 연관성 및 우선순위**
    - 특성들은 상호 연관되어 있으며 중요도에 따라 우선순위가 변동됨.
- **아키텍처 설계에 대한 인식**
    - **"아키텍처에서는 틀린 답은 없고 값비싼 답만 있다."**
        - 완벽한 답을 찾으려는 부담을 가질 필요 없음.
    - **"아키텍처에서 최고의 설계는 없다. 오직 나쁜 것 중에서 제일 나은 트레이드오프들만 있을 뿐!"**
- **특성의 우선순위 매기기**
    - 가장 단순한 필수 특성 세트로 정리해야 함.
    - 팀원들과 함께 덜 중요한 특성을 **선별**하는 훈련이 도움이 됨.
