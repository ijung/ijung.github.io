---
title: MODULAR MONOLITH - ARCHITECTURAL DRIVERS (모듈러 모놀로스 - 아키텍처 드라이버(아키텍처적 요구사항))
author: June
date: 2023-06-03 15:56:00 +0900
categories: [개발 게시물, MODULAR MONOLITH]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Kamil Grzybek](https://www.kamilgrzybek.com/about)의 포스트 [MODULAR MONOLITH: ARCHITECTURAL DRIVERS](https://www.kamilgrzybek.com/blog/posts/modular-monolith-architectural-drivers)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Introduction

[이전 포스트](https://ijung.github.io/posts/modular-monolith-a-primer/)에서는 *모듈러 모놀리스(Modular Monolith)*의 정의와 **모듈화**에 대한 설명에 초점을 맞췄습니다. 다시 한번, 모듈러 모놀리스는 다음과 같은 특징을 가집니다:

- 딱 하나의 배포 단위를 가진 시스템입니다.
- "모듈러(modular)" 방식으로 설계된 모놀리스 시스템에 대한 명확한 이름입니다.
- 모듈화에서의 모듈의 의미는 다음과 같습니다:
  - 독립적이고 자율적이어야 함을 의미합니다.
  - 원하는 기능을 제공하기 위해 필요한 모든 것을 가지고 있어야 합니다(비즈니스 영역별 분리).
  - 캡슐화되어 있고 잘 정의된 인터페이스/계약을 가지고 있습니다.
이 포스트에서는 제 생각에 가장 인기 있는 몇 가지 **아키텍처 드라이버(Architectural Drivers)**에 대해 이야기하고자 합니다. 이 드라이버들은 모듈러 모놀리스 또는 마이크로서비스 아키텍처로 이어질 수 있습니다.

그런데 *아키텍처 드라이버(Architectural Drivers)*은 무엇일까요?

## Architectural Drivers

일반적으로, X 아키텍처가 다른 아키텍처보다 더 나은지는 말할 수 없습니다. 모놀리스가 *마이크로서비스*보다 더 나은지, [클린 아키텍처](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)가 [레이어드 아키텍처](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)보다 더 나은지, 3개의 레이어가 4개의 레이어보다 더 좋거나 나쁜지 말할 수 없습니다.

이 같은 규칙은 ORM 대 raw SQL, "*현재 상태(Current State)*" 지속성 대 [이벤트 소싱](https://martinfowler.com/eaaDev/EventSourcing.html), [아네믹 도메인 모델](https://www.martinfowler.com/bliki/AnemicDomainModel.html) 대 리치 도메인 모델, 객체 지향 설계 대 함수형 프로그래밍 등 다른 고려사항에도 적용됩니다.

그렇다면, 불행히도 최고의 아키텍처/접근법/패러다임/도구/라이브러리가 없다면 어떻게 선택할 수 있을까요?

### Context is king

우리의 모든 **결정은 주어진 맥락(context)에서 이루어집니다**. 각 프로젝트는 서로 다르며(이는 프로젝트 정의에서 기인합니다), 따라서 각 맥락도 다릅니다. 이는 "동일한 결정이 한 맥락에서는 훌륭한 결과를 가져올 수 있지만, 다른 맥락에서는 참혹한 실패를 초래할 수 있다는 것"을 의미합니다. 이러한 이유로, 다른 사람이나 회사의 접근 방식을 **비판적 사고 없이** 사용하는 것은 많은 고통, 돈의 낭비, 그리고 결국 프로젝트의 종료를 초래할 수 있습니다.

|![Every Project is different and has different Context](/posts/development-contents/modular-monolith/Modular-Monolith_Contexts-1-1024x321.jpg)|
|:--:|
|Every Project is different and has different Context|
|모든 프로젝트는 다르고 컨텍스트가 다릅니다.|

그러나, 맥락은 너무 일반적인 개념이기 때문에 실제로 적용하기 위해서는 좀 더 구체적인 것이 필요합니다. 그래서 아키텍처 드라이버라는 개념이 정의되었습니다. Michael Keeling은 그의 [블로그 글](https://www.neverletdown.net/2014/10/architectural-drivers.html)에서 이에 대해 다음과 같이 서술하고 있습니다.

> Architectural drivers are formally defined as the set of requirements that have significant influence over your architecture.
>> 아키텍처 드라이버는 공식적으로 아키텍처에 중대한 영향을 미치는 요구사항 집합으로 정의됩니다.

Simon Brown은 '[Software Architecture for Developers](https://softwarearchitecturefordevelopers.com)'라는 책에서 아키텍처 드라이버를 비슷하게 설명합니다:

> Regardless of the process that you follow (traditional and plan-driven vs lightweight and adaptive), there’s a "set of common things that really drive, influence and shape the resulting software architecture".
>> 당신이 따르는 프로세스(전통적이고 계획 중심적인 것 vs 경량화하고 적응형인 것)에 상관없이, "결과적인 소프트웨어 아키텍처를 실제로 주도하고, 영향을 주고, 형성하는 일련의 공통된 요소들".

*아키텍처 드라이버*는 그들만의 분류를 가지고 있습니다. 주요 카테고리는 다음과 같습니다:

- 기능 요구사항 - 시스템이 어떤 문제를 어떻게 해결하는가
- 품질 속성 - 유지보수성이나 확장성과 같은 아키텍처의 품질을 결정하는 속성 집합
- 기술적 제약 - 기술 표준, 도구의 제한, 팀의 경험
- 비즈니스 제약 - 예산, 엄격한 마감일

|![Architectural Drivers](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Architectural-Drivers-1024x485.jpg)|
|:--:|
|Architectural Drivers|
|아키텍처 드라이버|

가장 중요한 것은 모든 *아키텍처 드라이버*들은 서로 연결되어 있으며, 종종 하나에 초점을 맞추면 다른 것이 손실되는 경우가 많다는 것입니다(불행하게도, 어디에서나 트레이드오프가 존재합니다). 예를 들어 설명하겠습니다.

*어떤 중요한 것을 계산하는 서비스가 있습니다(기능 요구사항). 그리고 이 계산은 3초가 걸립니다(품질 속성 - 성능). 새로운 요구사항이 나타나, 계산이 더 복잡해져서 이제 5초가 걸립니다(성능이 감소했습니다). 3초로 돌아가려면 다른 기술을 사용할 수 있지만, 그것을 위한 시간이 없습니다(비즈니스 제약 - 엄격한 마감일) 그리고 아직 회사에서 그것을 사용해 본 사람이 없습니다(기술적 제약 - 팀의 경험). 성능을 향상시키는 유일한 옵션은 계산을 저장 프로시저로 이동시키는 것인데, 이는 유지보수성과 가독성을 감소시킵니다(품질 속성).*

|![Architectural Drivers example](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Example-1024x444.jpg)|
|:--:|
|Architectural Drivers example|
|아키텍처 드라이버 예시|

보시다시피, **소프트웨어 아키텍처는 지속적인 선택**이며 한 가지 드라이버와 다른 드라이버 사이에서 계속해서 선택해야 합니다. '정답'이라는 것은 없습니다. [만능 해결책은 없습니다](https://en.wikipedia.org/wiki/No_Silver_Bullet).

이를 염두에 두고, *모듈러 모놀리스*와 *마이크로서비스* 아키텍처를 고려하며 논의되는 일반적인 *아키텍처 드라이버*와 속성들을 살펴보겠습니다.

## Level of Complexity

처음에, 분산 아키텍처와 비교했을 때 모듈러 모놀리스의 가장 큰 장점 중 하나인 복잡성을 고려해 봅시다. [위키](https://en.wikipedia.org/wiki/Complexity)에서의 복잡성(Complexity) 정의는 다음과 같습니다:

> Complexity characterizes the behavior of a system or model whose "components interact in multiple ways" and follow local rules, meaning there is no reasonable higher instruction to define the various possible interactions. The term is generally used to characterize something with many parts where those "parts interact with each other in multiple ways", culminating in a higher order of emergence greater than the sum of its parts.
>> 복잡성은 시스템이나 모델의 행동을 특징짓는데, "그 구성 요소들이 다양한 방법으로 상호작용하며" 지역 규칙을 따르는 것을 의미합니다. 즉, 다양한 가능한 상호작용을 정의할 수 있는 합리적인 상위 지시가 없습니다. 이 용어는 일반적으로 많은 부분들이 있고 "그 부분들이 서로 다양한 방법으로 상호작용하는 것"을 특징짓는 데 사용되며, 이는 그 부분들의 합보다 더 큰 순서의 출현을 초래합니다.

위에서 보시다시피, 복잡성은 구성 요소와 그들의 상호작용에 관한 것입니다. *모듈러 모놀리스* 아키텍처에서 모듈 간의 상호작용은 각 모듈이 같은 프로세스에 위치해 있기 때문에 단순합니다. 이는 다른 모듈과 상호작용하려는 모듈이:

- 요청을 보낼 정확한 주소를 알고 있으며, 이 주소가 변경되지 않을 것이라고 확신합니다
- 요청은 단지 메소드 호출일 뿐이며, 네트워크가 필요하지 않습니다
- 대상 모듈은 항상 사용 가능합니다
- 보안 문제는 걱정할 사항이 아닙니다

|![Modular Monolith Complexity](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Complexity-1024x405.jpg)|
|:--:|
|Modular Monolith Complexity|
|모듈러 모놀리스의 복잡성|

반면에, 분산 시스템 아키텍처를 생각해봅시다. 이 아키텍처에서, 모듈들/서비스들은 다른 서버에 위치해 있고 네트워크를 통해 통신합니다. 이는 서비스가 다른 서비스와 통신하려 할 때, 다음과 같은 문제들에 대처해야 한다는 것을 의미합니다:

- 대상 모듈의 주소를 어떻게든 얻어야 합니다, 왜냐하면 그것은 변할 수 있기 때문입니다
- 통신은 네트워크를 통해 이루어지며, 이는 HTTP와 같은 특별한 프로토콜과 직렬화의 사용을 필요로 합니다
- 네트워크가 사용할 수 없을 수 있습니다 ([CAP 이론](https://en.wikipedia.org/wiki/CAP_theorem))
- 모듈 간의 안전한 통신이 보장되어야 합니다

물론, 이런 문제들에 대한 해결책을 찾을 수 있습니다. 예를 들어, 주소 지정 문제를 해결하기 위해 [서비스 레지스트리(Service Registry)](https://microservices.io/patterns/service-registry.html)를 추가하고 [서비스 발견 패턴(Service Discovery pattern)](https://microservices.io/patterns/server-side-discovery.html)을 구현할 수 있습니다. 그러나, 이는 시스템에 더 많은 구성 요소와 알고리즘을 추가하는 것을 의미하므로 복잡성이 빠르게 증가합니다.

*마이크로서비스* 아키텍처가 발생시키는 문제의 규모를 인지하기 위해, 이 문제들을 해결하는 데 사용되는 [패턴](https://microservices.io/patterns/index.html)들에 익숙해지는 것을 추천합니다. 리스트는 크고, 그 중 대부분은 모놀리스 아키텍처에서는 전혀 필요하지 않습니다.

|![Complexity - Distributed System](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Complexity-Distribiuted-System-1024x405.jpg)|
|:--:|
|Complexity - Distributed System|
|복잡성 - 분산 시스템|

요약하자면, 모듈러 모놀리스의 아키텍처는 확실히 분산 시스템의 아키텍처보다 복잡성이 덜합니다. 높은 복잡성은 유지보수성, 가독성, 관찰 가능성을 감소시킵니다. 이는 경험 많은 팀, 고급 인프라, 특정 조직 문화 등이 필요합니다. 단순성이 당신의 주요 아키텍처 드라이버라면 [먼저 모놀리스(Monolith First)](https://martinfowler.com/bliki/MonolithFirst.html)를 고려해 보세요.

## Productivity

변경 사항을 제공하는 팀의 생산성은 두 가지 차원에서 측정될 수 있습니다: 전체 시스템(entire system)의 맥락에서와 단일 모듈(single module)의 맥락에서입니다.

전체 시스템의 맥락에서는 문제가 명확합니다. *모듈러 모놀리스*의 아키텍처는 복잡성이 적기 때문에 => 복잡성이 적을수록 이해하기 쉽기 때문에 => 생산성이 더 높습니다.

전체 시스템의 운영이 용이성 관점에서 보면, *모듈러 모놀리스*는 최대 수준의 생산성을 보장합니다 - 단지 코드를 다운로드 받아 로컬 기기에서 실행하면 됩니다. 분산 아키텍처에서는 이 프로세스를 용이하게 하는 기술과 도구들(예: Docker와 Kubernetes)에도 불구하고, 문제는 그렇게 단순하지 않습니다.

|![Running entire system - Monolith vs Distributed](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Productivity-1024x485.jpg)|
|:--:|
|Running entire system - Monolith vs Distributed|
|전체 시스템 실행 - 모놀리스 vs 분산|

반면에, 우리가 단일 모듈의 개발과 같은 생산성을 가지고 있다고 한다면, 이 경우에는 마이크로서비스 아키텍처가 더 나을 것입니다, 왜냐하면 우리는 특정 모듈을 테스트하기 위해 전체 시스템을 실행할 필요가 없기 때문입니다.

그럼 어떤 아키텍처가 팀의 생산성에 유리할까요? 제 생각에는, 대부분의 시스템에 대해 *모듈러 모놀리스*가 유리합니다. 하지 정말로 큰 프로젝트들(수십 또는 수백 개의 모듈들)에 대해서는 마이크로서비스가 유리할 것입니다. 만약 당신의 아키텍처 드라이버가 개발 속도이고 시스템이 거대하지 않다면, 더 나은 선택은 *모듈러 모놀리스*가 될 것입니다. 그리고 시스템이 확장될 경우, 마이크로서비스로 전환하는 것이 올바를 수 있습니다.

## Deployability

소프트웨어 시스템의 배포 가능성은 그것이 개발에서 제품으로 얼마나 쉽게 이동할 수 있는지에 관한 것입니다. 그러나, 여기서 우리는 2가지 상황을 고려해야 합니다. 전체 시스템의 배포와 단일 모듈의 배포입니다.

전체 시스템에 대한 맥락에서 보면, 여러 애플리케이션 중 하나를 배포하는 것이 더 쉬울까요? 당연히 하나의 애플리케이션을 배포하는 것이 더 쉽기 때문에 *모듈러 모놀리스*가 더 나은 옵션으로 보입니다.

|![Deployment - Modular Monolith](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Deployment-Modular-Monolith-1024x405.jpg)|
|:--:|
|Deployment - Modular Monolith|
|배포 - 모듈러 모놀리스|

그러나 반면에, *모듈러 모놀리스*에서는 항상 전체 시스템을 배포해야 합니다. 우리는 특정 모듈 하나만 배포할 수 없고, 이것은 가장 중요한 단점 중 하나입니다. 이 아키텍처에서는 배포 자율성이 없으므로 배포 과정이 조정되어야 하며, 이는 더 어려울 수 있습니다.

|![Deployability - Distribiuted System](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Deployment-Distribiuted-System-1024x405.jpg)|
|:--:|
|Deployability - Distribiuted System|
|배포 가능성 - 분산 시스템|

요약하자면, 만약, 전체 시스템의 배포에 상관없고, 배포의 자율성에 대해 신경쓰지 않는다면 이것은 *모듈러 모놀리스*의 핵심입니다. 그렇지 않다면, 분산 아키텍처를 고려해보세요.

## Performance

성능은 일반적으로 응답 시간, 처리 시간 또는 대기 시간과 같은 측면에서 어떤 것이 얼마나 빠른지에 대한 것입니다.

모든 요청이 순차적으로 처리되는 시나리오를 가정한다면, 모놀리스 아키텍처는 항상 분산 시스템보다 효율적일 것입니다. 모든 모듈은 동일한 프로세스에서 동작하므로, 그들 사이의 통신에 대한 오버헤드가 없습니다.

분산 시스템은 네트워크를 통한 통신에 의한 오버헤드가 있습니다 - 직렬화와 역직렬화, 암호화 및 패킷 전송 속도 등입니다.

실제 시나리오에서도 모놀리스는 더 효율적일 것이지만, 일정 시간 동안만 그럴 것입니다. 사용자, 요청, 데이터, 계산의 복잡성이 증가함에 따라 성능이 감소할 수 있습니다. 그러면 우리는 *마이크로서비스* 아키텍처의 주요 드라이버 중 하나인 확장성(scalability)에 도달하게 됩니다.

## Scalability

확장성이란 무엇인가요? [위키피디아](https://en.wikipedia.org/wiki/Scalability)에 따르면:

> Scalability is the property of a system to handle a growing amount of work by adding resources to the system
>> 확장성은 시스템에 리소스를 추가함으로써 증가하는 작업량을 처리하는 시스템의 성질입니다

다시 말해, 확장성은 소프트웨어가 더 많은 요청이나 데이터를 처리하는 능력에 관한 것입니다.

이것을 예를 들어 보여주는 것이 가장 좋습니다. 우리의 모듈 중 하나가 처음에 가정했던 것보다 더 많은 요청을 처리해야 한다고 가정해 봅시다. 이를 위해 이 모듈의 작동을 담당하는 리소스를 늘려야 합니다.

우리는 항상 두 가지 방법으로 이를 수행할 수 있습니다. 노드의 연산 능력을 늘리는 것(*수직 확장*이라고 함, scale up) 또는 새로운 노드를 추가하는 것(*수평 확장*이라고 함, scale out). 모놀리스와 *마이크로서비스* 아키텍처의 관점에서 이것이 어떻게 보이는지 살펴봅시다:

|![Scaling](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Scaling-954x1024.jpg)|
|:--:|
|Scaling|
|스케일링(크기 조정)|

위에서 볼 수 있듯이, 두 아키텍처 모두 확장할 수 있습니다. 모놀리스도 확장할 수 있습니다. 수직 확장은 동일하지만, 수평 확장에서 차이점이 있습니다. 이 접근 방식을 사용하면, *모듈러 모놀리스*는 전체적으로만 확장할 수 있어, 리소스 활용이 비효율적이게 됩니다. *마이크로서비스* 아키텍처에서는, **우리가 확장해야 할 모듈만 확장**하므로, 리소스 활용이 더 효율적입니다. 이것이 주요 차이점입니다.

모듈의 인스턴스가 더 많이 작동해야 할수록 차이점이 더욱 두드러집니다. 반면에, 많이 확장할 필요가 없다면, 아마도 리소스 활용이 덜 효율적인 것을 받아들이고 모놀리스를 유지하며 그 외의 장점을 가져가는 것이 더 좋을지도 모릅니다. 이는 이러한 상황에서 우리가 스스로에게 물어야 할 좋은 질문입니다.

## Failure impact

가끔 우리의 아키텍처 드라이버는 실패의 영향을 제한하는 것일 수 있습니다. 가령, 우리가 가끔씩 전체 프로세스를 다운시키는 매우 불안정한 모듈을 가지고 있다고 가정해봅시다.

*모듈러 모놀리스*의 경우, 전체 시스템이 하나의 프로세스에서 작동하기 때문에, 전체 시스템이 갑자기 작동을 멈추고 우리의 가용성이 감소하게 됩니다.

*마이크로서비스* 아키텍처의 경우, "위험한(risky)" 모듈을 별도의 프로세스로 이동시킬 수 있으며, 이 경우 해당 모듈이 멈추더라도 시스템의 나머지 부분은 제대로 작동할 것입니다.

|![Failure impact](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Failure-impact-1024x405.jpg)|
|:--:|
|Failure impact|
|실패의 영향|

*모듈러 모놀리스*의 가용성을 높이려면 노드의 수를 늘릴 수 있지만, 스케일링과 마찬가지로 리소스 활용성은 *마이크로서비스* 아키텍처에 비해 최고 수준이 아닐 것입니다.

## Heterogeneous Technology

*모듈러 모놀리스*의 특성 중 하나는 **다양한 기술**을 사용할 수 없다는 점입니다. 전체 시스템이 동일한 프로세스에서 실행되기 때문에, 이는 "같은 런타임 환경에서 실행되어야 한다"는 의미입니다. 이것은 시스템이 반드시 같은 언어로 작성되어야 한다는 의미는 아닙니다. 왜냐하면 몇몇 플랫폼은 여러 언어를 지원하기 때문입니다(예를 들어, .NET CLR이나 JAVA JVM). 그러나 완전히 분리된 기술을 사용하는 것은 불가능합니다.

|![Heterogeneous Technology](/posts/development-contents/modular-monolith/Modular-Monolith_-Architectural-Drivers-Heterogeneous-Technology-1024x347.jpg)|
|:--:|
|Heterogeneous Technology|
|이종 기술|

이종 기술의 특징은 *마이크로서비스* 아키텍처로 전환하는 데 결정적인 요소가 될 수 있지만, 반드시 그럴 필요는 없습니다. 종종 회사들은 하나의 기술 스택을 사용하고, 팀의 역량이나 소프트웨어 라이센스 때문에 다른 기술로 구성 요소를 구현하는 것조차 생각하지 않습니다.

반면에, 더 큰 회사들과 프로젝트들은 특정 문제를 해결하기 위해 맞춤형 도구를 사용하여 생산성을 극대화하기 위해 더 자주 다른 기술들을 사용합니다.

이종 기술과 관련된 일반적인 경우는 레거시 시스템의 유지보수와 개발입니다. 레거시 시스템은 종종 오래된 기술로 작성되며(그리고 종종 매우 나쁜 방식으로 작성됩니다). 새로운 기술을 사용하기 위해, 새로운 기능을 구현하는 새로운 서비스/시스템이 종종 생성되고, 오래된 시스템은 새로운 시스템에 요청을 위임만 합니다. 이 덕분에 레거시 시스템의 개발이 더 빠르게 이루어질 수 있고, 레거시 시스템을 운영 인력을 더 쉽게 찾을 수 있습니다. 여기서의 단점은 하나 대신 두 시스템이 있기 때문에 - 전체 시스템이 분산되어 - 분산된 시스템 아키텍처의 모든 단점들이 적용됩니다.

## Summary

이 게시물은 *모듈러 모놀리스* 또는 *마이크로서비스*를 선호하는 모든 아키텍처 드라이버를 설명하려는 것이 아닙니다. 이 주제에 대해 별도의 책이 작성되고 있습니다.

이 게시물에서는 가장 흔히 논의되는 아키텍처 드라이버(제 의견으로는)를 설명하고, **우리 시스템의 아키텍처 형태는 많은 요소에 의해 영향을 받으며, 모든 것이 맥락(context)에 따라 달라진다**는 것을 매우 명확하게 하고 싶었습니다.

요약:

- 더 좋거나 나쁜 아키텍처는 없습니다 - 모든 것은 맥락(context)과 아키텍처 드라이버에 달려 있습니다
- 아키텍처 드라이버는 기능 요구 사항, 품질 속성, 기술 제약사항, 비즈니스 제약사항으로 분류됩니다
- *모놀리스* 아키텍처는 분산 시스템보다 복잡성이 적습니다. *마이크로서비스* 아키텍처는 훨씬 더 많은 도구, 라이브러리, 컴포넌트, 팀 경험, 인프라 관리 등이 필요합니다
- 처음에는 *모놀리스* 구현이 더 생산적일 것입니다 (모놀리스 먼저 접근 방식). 나중에는 마이크로서비스 아키텍처로의 마이그레이션을 고려할 수 있지만, 그 마이그레이션에 대한 아키텍처 드라이버가 존재하는 경우에만 그렇습니다
- *모놀리스*의 배포는 더 쉽지만 자율적인 배포를 지원하지 않습니다.
- 두 아키텍처 모두 확장성을 지원하지만, *마이크로서비스*는 훨씬 더 효율적입니다 (자원 활용)
- *모놀리스*는 스케일링이 필요할 때까지 *마이크로서비스*보다 성능이 더 좋습니다 - 그 후에는 스케일링 가능성에 달려 있습니다
- 장애 영향은 *모놀리스*에서 더 큽니다 왜냐하면 모든 것이 같은 프로세스에서 작동하기 때문입니다. 위험은 복제로 완화될 수 있지만, 이것은 *마이크로서비스* 아키텍처보다 더 비용이 들 것입니다
- *모놀리스*는 원칙적으로 이종 기술을 지원하지 않습니다

## Additional Resources

1. [Architectural Drivers - chapter from Designing Software Architectures: A Practical Approach book - Humberto Cervantes, Rick Kazman.](http://www.informit.com/articles/article.aspx?p=2738304&seqNum=4)
1. [Software Architecture for Developers book - Simon Brown.](https://softwarearchitecturefordevelopers.com/)
1. [Design It! book - Michael Keeling.](https://www.oreilly.com/library/view/design-it/9781680502923/)
1. [Collection of articles about Monolith and Microservices architectures named “When microservices fail…“.](https://docs.google.com/spreadsheets/d/1vjnjAII_8TZBv2XhFHra7kEQzQpOHSZpFIWDjynYYf0/edit#gid=0)
1. [Modular Monolith with DDD – GitHub repository](https://github.com/kgrzybek/modular-monolith-with-ddd)
