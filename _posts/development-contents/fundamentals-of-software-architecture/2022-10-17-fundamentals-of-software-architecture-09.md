---
title: 소프트웨어 아키텍처 101 - CHAPTER 9 기초
author: June
date: 2024-12-20 14:09:00 +0900
categories: [개발 책, 소프트웨어 아키텍처 101]
tags: [공부, 책, 소프트웨어 아키텍처 101]
toc: true
math: true
mermaid: true
comments: true
---

아키텍처 스타일은 종종 아키텍처 패턴이라고 부르며, 다양한 아키텍처 특성을 다루는 컴포넌트의 명명된 관계(named relationship)를 기술합니다. 아키텍처 스타일의 명칭은 (설계 패턴도 그렇듯이) 숙련된 아키텍트들 사이에서 간명하게 지칭할 수 있는 이름으로 붙여 놓았습니다. 따라서 아키텍트는 기초적인 아키텍처 스타일의 명칭에 익숙해져야 합니다.

아키텍처 스타일은 각 명칭마다 설계 패턴의 존재 이유이기도 한, 상당히 많은 세부 내용이 함축되어 있습니다. 또 아키텍처 스타일은 토폴로지와 기본 전제된 아키텍처 특성을, 이로운 것과 해로운 것 모두 기술합니다. 아키텍트는 규모가 더 큰 패턴에서 나타나는 몇 가지 근본적인 패턴들을 꿰고 있어야 합니다.

# 9.1 기초 패턴

## 9.1.1 진흙잡탕

뭐 하나 뚜렷한 아키텍처 구조가 전무한 상태를 진흙잡탕(Big Ball of Mud)이라고 표현합니다.

진흙잡탕은 요즘에는 보통 실제 내부 구조라 할 만한 것은 하나도 없는, 데이터베이스를 직접 호출하는 이벤트 핸들러를 가진 단순한 스크립팅 애플리케이션을 가리킵니다. 보통 이렇게 별 대수롭지 않게 시작한 애플리케이션이 나중에 점점 규모가 커지면서 처치 곤란한 상태가 됩니다.

안타깝게도 이 아키텍처 안티패턴은 실제로 아주 흔히 발생합니다.

## 9.1.2 유니터리 아키텍처

유니터리(unitary, 단일, 통일) 시스템은 임베디드 시스템과 그 밖에 매우 제약이 많은 극소수 환경을 제외하면 거의 쓰이지 않습니다. 소프트웨어 시스템은 시간이 지날수록 점점 기능이 늘어나기 마련이므로 성능, 확장 등의 운영 아키텍처 특성을 유지하려면 관심사를 분리할 필요가 있습니다.

## 9.1.3 클라이언트/서버

프론트엔드와 백엔드로 기술적으로 기능을 분리한 2티어(two-tier) 또는 클라이언트/서버 아키텍처는 대표적인 기본 아키텍처 스타일입니다.

### 데스크톱 + 데이터베이스 서비

이 아키텍처는 표준 네트워크 프로토콜을 통해 접속 가능한 스탠드얼론(standalone, 단독형, 독립형) 데이터베이스 서버와 잘 맞았습니다. 덕분에 프레젠테이션 로직은 데스크톱에 두고 (양과 복잡도 모두) 계산량이 많은 액션은 사양이 탄탄한 데이터베이스 서버에서 실행했습니다.

### 브라우저 + 웹 서버

현재 웹 개발 시대가 도래하면서 웹 브라우저가 웹 서버에 접속하는 (그리고 웹 서버는 다시 데이터베이스 서버에 접속하는) 형태로 분리하는 것이 일반화됐습니다. 이로써 클라이언트는 데스크톱보다 훨씬 가벼운 브라우저로 대체되었고 내외부 방화벽 모두 더 넓은 범위로 배포가 가능해졌습니다.

### 3티어

고성능 데이터베이스 서버를 사용하는 데이터베이스 티어, 애플리케이션 서버가 관리하는 애플리케이션 티어, 그리고 처럼에는 HTML로 시작하여 기능이 점점 많아져 온갖 자바스크립트 코드로 가득 찬 프론트엔드 티어, 이렇게 세 티어가 완성됐습니다.

# 9.2 모놀리식 대 분산 아키텍처

아키텍처 스타일은 크게 (전체 코드를 단일 단위로 배포하는) 모놀리식과 (원격 액세스 프로토콜을 통해 여러 단위로 배포하는) 분산형 두 종류입니다.

분산 아키텍처 스타일은 모놀리식 아키텍처 스타일에 비해 성능, 확장성, 가용성 측면에서 훨씬 강력하지만, 이런 파워에도 결코 무시할 수 없는 트레이드오프가 수반됩니다.

<aside>
💡 `the fallacies of distributed computing: 분산 컴퓨팅의 오류(https://oreil.ly/fVAEY)`
1994년 L. 피터 도이치(Peter Deutsch) 및 그와 함께 선마이크로시스템즈에서 일했던 동료들이 작성한 글에서 모든 분산 아키텍처에서 맞닥뜨리게 되는 이슈들이 거론되어 있다. 여기서 오류(fallacy)는 옳다고 믿거나 가정하지만 실은 틀린 것을 말하며, 오늘날의 분산 아키텍처에서도 이 8가지 오류가 적용된다.

</aside>

## 9.2.1 오류 #1: 네트워크는 믿을 수 있다

개발자, 아키텍트 모두 네트워크는 믿을 수 있다고 전제하지만 실제로는 전혀 그렇지 않습니다. 그래서 타임아웃(timeout) 같은 장치를 마련하거나 서비스 사이에 회로 차단기(circuit breaker, 서킷 브레이커)를 두는 것입니다. 시스템이 (마이크로 아키텍처처럼) 네트워크에 더 의존할 수록 시스템의 신뢰도는 잠재적으로 떨어질 가능성이 있습니다.

## 9.2.2 오류 #2: 레이턴시는 0이다

모든 분산 아키텍처에서 레이턴시는 0이 아닙니다.

아키텍트는 어떤 분산 아키텍처를 구축하든지 간에 평균 레이턴시는 반드시 알아야 합니다. 이 것이 분산 아키텍처가 실현 가능한지 판단하는 유일한 방법입니다. 평균 레이턴시도 중요하지만 95~99번째 백분위수(percentile)를 이해하는 것은 더 중요합니다. ‘긴 꼬리(long tail)’ 레이턴시가 분산 아키텍처의 성능을 저해하는 주범이 됩니다.

## 9.2.3 오류 #3: 대역폭은 무한하다

마이크로 분산 아키텍처에서 시스템이 자잘한 배포 단위(서비스)로 쪼개지면 이 서비스들 간에 주고받는 통신이 대역폭을 상당히 점유하여 네트워크가 느려지고, 결국 레이턴시와 신뢰성에도 영향을 미칩니다.

<aside>
💡 `스템프 커플링(Stammp Coupling)`

스템프 커플링(Stamp Coupling)은 소프트웨어 공학에서 특히 분산 시스템 아키텍처나 모듈 간의 결합에 대해 논의할 때 사용되는 개념으로, **하나의 모듈이 다른 모듈과 상호작용할 때 특정 데이터 구조(예: 레코드, 객체 등)를 전달함으로써 발생하는 높은 결합도를 의미**합니다. 이는 모듈 간의 데이터 의존성이 강하다는 것을 나타내며, 이러한 의존성은 시스템의 유지보수성과 확장성을 저하시킬 수 있습니다.

</aside>

스탬프 커플링(stamp coupling)은 분산 아키텍처에서 상당히 많은 대역폭을 차지합니다. 스템프 커플링은 다음과 같은 방법으로 해결할 수 있습니다.

- 프라이빗 REST API 엔드포인트를 둔다.
    - 프라이빗 REST API 엔드포인트는 외부에 공개되지 않은 내부 시스템 간의 통신을 위한 API 엔드포인트입니다. 이 방법은 내부 모듈들이 공용 API를 공유하지 않고, 특정 데이터만을 주고받도록 설계하여 결합도를 낮춥니다.
- 계약에 필드 셀렉터(field selector)를 사용한다.
    - 필드 셀렉터를 사용하면 클라이언트가 필요한 데이터 필드만 선택해서 가져올 수 있습니다. 이는 전송되는 데이터의 양을 줄여 대역폭을 절약하고, 모듈 간의 결합도를 낮추는 데 도움이 됩니다.
- GraphQL로 계약을 분리한다.
    - GraphQL을 사용하면 클라이언트가 필요한 데이터만 요청할 수 있으며, 특정한 데이터 구조에 대한 의존성이 줄어듭니다. GraphQL을 통해 클라이언트는 자신에게 필요한 데이터 조각만 요청할 수 있기 때문에 스템프 커플링 문제를 완화할 수 있습니다.
- 컨슈머 주도 계약(Consumer-Driven Contract, CDC)와 값 주도 계약(Value-Driven Contract, VBC)을 병용한다.
    - 컨슈머 주도 계약(CDC)은 서비스의 소비자(클라이언트)가 자신이 필요로 하는 서비스의 인터페이스를 정의하고, 서비스 제공자가 이 계약을 준수하는 방식입니다. 값 주도 계약(VBC)은 데이터 계약을 기반으로 하여 데이터 형식과 내용의 일관성을 보장하는 방법입니다. CDC와 VBC를 병용하면 데이터 구조 변경 시에도 소비자와 공급자 간의 결합도를 최소화하면서 일관성을 유지할 수 있습니다.
- 내부 메시징 엔드포인트를 사용한다.
    - 내부 메시징 엔드포인트는 서비스 간의 직접적인 데이터 요청 대신 메시지 큐나 이벤트 버스를 통해 비동기적으로 데이터를 주고받는 방식입니다. 이는 서비스 간의 결합도를 낮추고, 데이터 전송 효율성을 높입니다.

## 9.2.4 오류 #4: 네트워크는 안전하다

분산 배포된 각 엔드포인트는 알려지지 않은, 또는 악의적인 요청이 해당 서비스로 유입되지 않게 철저한 보안 대책을 강구해야 합니다. 모든 엔드포인트에, 서비스 간 통신에도 보안이 적용돼야 하므로 마이크로서비스나 서비스 기반 아키텍처러럼 고도로 분산된 동기 아키텍처에서 당연히 성능이 떨어질 수 밖에 없습니다.

## 9.2.5 오류 #5: 토폴로지는 절대 안 바뀐다

네트워크를 구성하는 모든 라우터, 허브, 스위치, 방화벽, 네트워크, 어플라이언스(appliance) 등 전체 네트워크 토폴로지가 불변일 거란 가정은 섣부른 오해입니다. 네트워크 토폴로지는 가만히 있질 않습니다. 당연히 변합니다!

아키텍트는 운영자, 네트워크 관리자와 항시 소통을 하면서 무엇이, 언제 변경되는지 알고 있어야 합니다.

## 9.2.6 오류 #6: 관리자는 한 사람 뿐이다

대기업에서 일하는 네트워크 관리자는 보통 수십 명에 이릅니다. 그래서 분산 아키텍처는 복잡할 수 밖에 없고, 모든 것을 정상 궤도에 올려놓으려면 상당히 많은 조율 과정이 불가피합니다. 모놀리식 애플리케이션은 단일 단위로 배포하기 때문에 이 정도의 소통과 협력까지는 필요하지 않습니다.

## 9.2.7 오류 #7: 운송비는 0이다

여기서 운송비는 레이턴시가 아니라, ‘단순한 REST 호출’을 하는 데 소모되는 진짜 비용(actual cost)을 말합니다. 분산 아키텍처는 하드웨어, 서버, 게이트웨이, 방화벽, 신규 서브넷, 프록시 등 리소스가 더 많이 동원되므로 모놀리식 아키텍처보다 비용이 훨씬 더 듭니다.

## 9.2.8 오류 #8: 네트워크는 균일하다

실제로 많은 회사의 인프라는 여러 업체의 네트워크 하드웨어 제품들이 얽히고 설켜 있습니다.

요는, 온갖 종류의 하드웨어가 서로 다 잘 맞물려 동작하는 건 아니라는 것힙니다. 다시 말해, 이 오류는 다른 오류들로 되돌아가 (분산 아키텍처를 사용할 때 반드시 필요한) 네트워크에 대한 끝없는 혼란과 당혹스러움을 야기할 수 있음을 시사합니다.

## 9.2.9 다른 분산 아키텍처 고려 사항

### 분산 로깅

분산 아키텍처는 애플리케이션과 시스템 로그가 분산되어 있으므로 어떤 데이터가 누락된 근본 원인을 밝혀내기가 대단히 어렵고 시간도 많이 걸립니다.

### 분산 트랜잭션

분산 아키텍처는 최종 일관성(eventual consistency)이라는 개념을 바탕으로 별도로 분리된 배포 단위에서 처리된 데이터를 미리 알 수 없는 어느 시점에 모두 일관된 상태로 동기화합니다. 확장성, 성능, 가용성을 얻는 대가로, 데이터 일관성과 무결성을 희생하는 트레이드오프인 셈이죠.

### 계약 관리 및 버저닝

<aside>
💡 `계약`은 클라이언트와 서비스 모두 합의한 행위와 데이터입니다.

</aside>

분산 아키텍처에서는 분리된 서비스와 시스템을 제각기 다른 팀과 부서가 소유하기 때문에 계약 유지보수가 특히 어렵습니다.

# 요약

## 9.1 기초 패턴

### **9.1.1 진흙잡탕 (Big Ball of Mud)**

- 뚜렷한 아키텍처 구조가 없고, 데이터베이스를 직접 호출하는 이벤트 핸들러로 구성된 단순 스크립팅 애플리케이션.
- 작은 규모로 시작했다가 점차 커지면서 복잡해지는 경향.
- **9.1.2 유니터리 아키텍처 (Unitary Architecture)**
    - 임베디드 시스템과 같이 제약이 많은 환경을 제외하면 거의 사용되지 않음.
    - 기능이 늘어남에 따라 관심사를 분리할 필요가 있는 구조.

### **9.1.3 클라이언트/서버 (Client/Server)**

- 프론트엔드와 백엔드를 분리한 2티어 또는 클라이언트/서버 아키텍처.
- **데스크톱 + 데이터베이스 서버:** 스탠드얼론 데이터베이스 서버와 데스크톱 클라이언트를 사용하는 구조.
- **브라우저 + 웹 서버:** 웹 브라우저와 웹 서버가 데이터를 주고받는 형태.
- **3티어:** 데이터베이스 티어, 애플리케이션 티어, 프론트엔드 티어로 구성된 구조.

## 9.2 모놀리식 대 분산 아키텍처

- **모놀리식 아키텍처 (Monolithic Architecture):**
    - 전체 코드를 단일 단위로 배포.
- **분산 아키텍처 (Distributed Architecture):**
    - 원격 액세스 프로토콜을 통해 여러 단위로 배포.
    - 성능, 확장성, 가용성 측면에서 강력하지만, 트레이드오프가 있음.
- **분산 컴퓨팅의 오류 (Fallacies of Distributed Computing):**
    - **오류 #1:** 네트워크는 믿을 수 없다.
    - **오류 #2:** 레이턴시는 0이다.
    - **오류 #3:** 대역폭은 무한하다.
    - **오류 #4:** 네트워크는 안전하다.
    - **오류 #5:** 토폴로지는 절대 안 바뀐다.
    - **오류 #6:** 관리자는 한 사람 뿐이다.
    - **오류 #7:** 운송비는 0이다.
    - **오류 #8:** 네트워크는 균일하다.
- **다른 분산 아키텍처 고려 사항:**
    - **분산 로깅:** 애플리케이션과 시스템 로그가 분산되어 있어 분석이 어려움.
    - **분산 트랜잭션:** 최종 일관성(eventual consistency) 개념을 사용, 데이터 일관성을 트레이드오프.
    - **계약 관리 및 버저닝:** 분리된 서비스와 시스템 간의 계약 유지보수가 어려움.
    