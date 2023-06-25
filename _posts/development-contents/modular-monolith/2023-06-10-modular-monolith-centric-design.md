---
title: MODULAR MONOLITH - DOMAIN-CENTRIC DESIGN (모듈러 모놀로스 - 도메인 중심 설계)
author: June
date: 2023-06-10 12:00:00 +0900
categories: [개발 게시물, MODULAR MONOLITH]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Kamil Grzybek](https://www.kamilgrzybek.com/about)의 포스트 [https://www.kamilgrzybek.com/blog/posts/modular-monolith-domain-centric-design](https://www.kamilgrzybek.com/blog/posts/modular-monolith-domain-centric-design)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Introduction

이 시리즈의 이전 글들에서, 저는 *모듈러 모놀리스*가 무엇인지, 그 구조는 어떻게 생겼는지, 그리고 이 구조가 어떻게 강제되는지에 대해 다루었습니다. 그 다음으로, 이 구조를 위한 아키텍처 드라이버와 모듈 간의 통합 스타일에 대해 설명했습니다.

이 글에서는 저는 더 깊게 들어가려고 합니다 - 한 단계 더 아래로 내려가서 이러한 아키텍처가 어떻게 설계될 수 있는지 설명하려고 합니다. 우리는 아직 이 아키텍처를 구현하려는 것은 아닙니다 - 우리는 그 기술 중립적인 설계에 초점을 맞추려고 합니다.

## Domain-Centric Design

*모듈러 모놀리스* 아키텍처의 이름이 제안하는 것처럼, 우리의 아키텍처 설계는 높은 [모듈성](https://en.wikipedia.org/wiki/Modularity)을 목표로 진행되어야 합니다. 이로 인해 시스템은 **전체 비즈니스 기능**을 제공하는 **독립적인** 모듈을 가져야 합니다. 이것이 바로 도메인 중심 아키텍처와 디자인이 그 경우에 자연스러운 선택이 되는 이유입니다.

게다가, 우리가 알다시피, 모듈은 **잘 정의된 인터페이스**를 가져야 합니다. 모듈 간의 모든 통신은 이러한 인터페이스를 통해서만 이루어져야 하며, 이는 각 모듈이 높은 **캡슐화**를 가져야 함을 의미합니다.

이러한 아키텍처가 고수준에서 어떻게 보일 수 있는지 살펴보겠습니다:

|![Modular Monolith: Domain-Centric Design](/posts/development-contents/modular-monolith/ModularMonolithDesign.jpg)|
|:--:|
|Modular Monolith: Domain-Centric Design|
|모듈러 모놀리스: 도메인 중심 설계|

고수준의 관점에서 보면, *도메인 중심* 아키텍처와의 유사성이 있습니다. 이는 전체 시스템의 아키텍처([시스템 아키텍처](https://en.wikipedia.org/wiki/Systems_architecture))와 나중에 설명될 개별 모듈([애플리케이션 아키텍처](https://en.wikipedia.org/wiki/Applications_architecture)) 모두에 대해 정확히 그러한 경우입니다.

[육각형 아키텍처(Hexagonal Architecture)](https://alistair.cockburn.us/hexagonal-architecture/)와의 유사성을 보면, 다음과 같습니다:

- API: 주요 어댑터
- 모듈 API: 주요 포트
- 보조 포트와 그 어댑터들 (데이터베이스, 이벤트 버스, 다른 모듈과의 통신을 위해)

|![Modular Monolith: Hexagonal Architecture View](/posts/development-contents/modular-monolith/MMDesign_Hexagonal-1.jpg)|
|:--:|
|Modular Monolith: Hexagonal Architecture View|
|모듈러 모놀리스: 육각형 아키텍처 뷰|

세밀히 살펴보면, 이 아키텍처는 [어니언(Onion) 아키텍처](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1), [클린(Clean) 아키텍처](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)와 다르지 않습니다. 가장 중요한 것은 우리의 도메인이 [내부]에 있고, *의존성 규칙*이 준수된다는 것입니다:

> Source code dependencies must [point only inward], [toward higher-level policies].
>> 소스 코드 종속성은 [내부만 가리켜], [상위 정책을 향해야] 합니다.

|![Modular Monolith: Clean/Onion Architecture View](/posts/development-contents/modular-monolith/MMDesign_Clean_Onion.jpg)|
|:--:|
|Modular Monolith: Clean/Onion Architecture View|
|모듈러 모놀리스: 클린/어니언 아키텍처 뷰|

하나씩 모든 요소를 설명해봅시다.

### API

API는 우리 시스템의 진입점입니다. 주로 웹 서비스(SOAP/REST/GraphQL)로 구현되며 HTTP 요청을 받아들이고 HTTP 응답을 반환합니다.

API의 주요하고 유일한 책임은 요청을 적절한 모듈로 전달하는 것입니다. 이는 마이크로서비스 아키텍처의 [API 게이트웨이](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern)와 동일하지만, 서비스로의 네트워크 호출 대신에 [메모리 내의 모듈 호출]이 있습니다.

API는 매우 얇아야 합니다. 로직이 없어야 합니다 - 애플리케이션 논리나 비즈니스 논리 모두 없어야 합니다. 여기에 넣는 모든 것은 HTTP 요청 처리와 라우팅과 관련이 있어야만 합니다.

### Module

**각 모듈은 별도의 애플리케이션으로 취급되어야 합니다**. 다시 말해, 이는 우리 시스템의 하위 시스템입니다. 이 덕분에, 이는 [자율성]을 갖게 됩니다. 이는 다른 모듈(하위 시스템)과 느슨하게 또는 전혀 결합되지 않을 것입니다. 이는 각 모듈이 별도의 팀에 의해 개발될 수 있다는 것을 의미합니다. 이는 마이크로서비스 아키텍처의 경우와 같은 [아키텍처 드라이버](https://www.informit.com/articles/article.aspx?p=2738304&seqNum=4)입니다.

게다가, 우리는 특정 모듈을 별도의 런타임 컴포넌트로 쉽게 추출할 수 있을 것입니다 (모노리스 분할). 물론 필요한 경우에 한정된 이야기이며, 이는 우리의 아키텍처의 목표가 아니라, 모듈성의 훌륭한 부수 효과입니다.

모듈이 도메인 지향적이어야 한다면(DDD 전략 패턴 세트에서 [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html) 개념을 참조하세요), 우리는 모듈 자체의 수준에서 다시 도메인 중심 아키텍처를 사용할 수 있습니다.

모듈 아키텍처는 다음과 같습니다:

|![Module Architecture](/posts/development-contents/modular-monolith/Module_Architecture-1.jpg)|
|:--:|
|Module Architecture|
|모듈러 아키텍처|

#### Module Startup API

*모듈 시작 API*는 주어진 모듈이 초기화될 수 있도록 하는 *포트/인터페이스*입니다. 주어진 모듈이 독립적이어야 하므로, 그것은 자신을 초기화할 수 있어야 하며, 그 운영에 필요한 적절한 구성 매개변수만 받아야 합니다. 이는 우리가 API(또는 다른 모듈 호스트)에서 주어진 모듈을 구성하지 **않는다**는 것을 의미합니다. 우리는 시작시에만 초기화를 시작합니다.

#### Composition Root

모듈 자율성을 지원하는 것은 또한 주어진 모듈이 [자체적으로 객체 의존성 그래프를 생성]할 수 있어야 한다는 것을 의미합니다, 즉, 그것은 자신의 [컴포지션 루트](https://blog.ploeh.dk/2011/07/28/CompositionRoot/)를 가져야 합니다.

이것은 보통 그것이 자체 [IoC](https://en.wikipedia.org/wiki/Inversion_of_control) 컨테이너를 가지게 될 것이라는 것을 의미합니다. 이것은 매우 중요한 사항입니다. 불행하게도, 가장 흔한 접근법은 전체 런타임 구성 요소(component)별로 정의된 IoC 컨테이너입니다. 이는 작은 시스템에 대한 좋은 접근법이지만, 더 복잡하고 모듈화된 시스템에 대해서는 그렇지 않습니다.

#### Module API

모듈 API는 주어진 모듈과의 통신(초기화 제외 - *모듈 시작 API* 참조)을 위한 인터페이스(기본 포트)입니다. 이런 모듈 API는 두 가지 방법으로 생성될 수 있습니다:

- 전통적인 접근법: 메소드의 목록 (`CustomerService.GetCustomer, OrderService.AddOrder`)
- *[CQRS](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs) 스타일* 접근법: 보내야 할 쿼리와 명령의 세트 (`GetCustomerQuery, AddOrderCommand`)

저는 확실히 두 번째 *CQRS 스타일* 접근법의 팬이지만, 제 생각에는 첫 번째 접근법도 충분히 허용됩니다.

[모듈화의 주요 속성들](https://en.wikipedia.org/wiki/Modular_programming)에 따르면, 이 모듈 API는 가능한 한 작아야 합니다 - 필요한 것만 공개합니다(더 적지도, 더 많지도 않게). 이렇게 하면 더 [안정적](https://wiki.c2.com/?StableDependenciesPrinciple)이게 됩니다.

#### Infrastructure - Secondary Adapters

이 곳에서 2차 어댑터의 구현이 있어야 합니다(포트 앤드 어댑터 아키텍처의 명명법에서 따옴). 2차 어댑터는 데이터베이스, 이벤트 버스, 다른 모듈 등 외부 의존성(프로세스 내 및 프로세스 간)과의 통신을 담당합니다.

#### Application

여기서는 모듈과 관련된 유스케이스의 구현을 찾아야 합니다. 이는 *Hexagon* 경계(포트 앤드 어댑터 아키텍처 관점에서), *Application Core* 경계(Onion Architecture 아키텍처 관점에서), 또는 *Use Cases* 레이어(Clean Architecture 관점에서)입니다.

어떤 도메인 중심 아키텍처를 따르더라도 원칙은 같습니다. 이곳에서는 더 이상 기술적인 문제와 인프라 문제를 다루지 않습니다. 여기서는 애플리케이션과 비즈니스 요구 사항의 구현에만 집중합니다.

그러나, 때때로 도메인이 간단하지 않아 이 레이어를 *도메인*이라고 명시적으로 분리하고 싶을 때가 있습니다.

#### Domain

가장 복잡하지 않은 모듈조차도 도메인이 있습니다. 그리고 여기서 도메인이 가장 중요합니다. 모듈러 모노리스 아키텍처와 마찬가지로 모든 도메인 중심 아키텍처의 핵심입니다. 도메인 중심 아키텍처 덕분에, 이는 프레임워크와 인프라에서 분리됩니다.

이 도메인의 모델([Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html))은 Bounded Context(경계)에서만 적용 가능합니다. 여기에는 우리 도메인과 관련된 개념과 소위 *기업 비즈니스 규칙*만이 있을 것입니다.

도메인 모델은 [Persistence Ignorance](https://www.kamilgrzybek.com/blog/posts/domain-model-encapsulation-ef)를 가져야 합니다. [Ubiquitous Language](https://martinfowler.com/bliki/UbiquitousLanguage.html)로 작성되어 있고 완전히 테스트 가능해야 합니다. 여기에 가장 많이 집중하며, 나머지는 우리가 일을 더 쉽게 하도록 도와줍니다.

#### How many layers?

인터넷에는 애플리케이션 계층에 대한 많은 논의가 있습니다. 일부는 매우 명확한 계층 분할을 선호합니다(예: 별도의 라이브러리/패키지 또는 다른 언어 기술 사용). 다른 사람들은 논리적 분해 없이 모든 것을 함께 유지하려고 합니다.

먼저, **각 모듈이 다를 것임을 알아두세요. 하나는 도메인이 더 복잡할 수 있고, 다른 하나는 CRUD 연산만을 구현할 수 있습니다**. 이 경우, 이러한 모듈들에 대한 애플리케이션 아키텍처는 다를 것입니다.

또한, 동일한 모듈 내에서도 더 복잡하고 덜 복잡한 기능이 모두 있을 수 있습니다. 이 경우, 우리는 각 기능을 별도로 존중해야 합니다.

결론적으로, 계층의 적용은 주어진 모듈에 대한 전역적인 결정이어서는 안됩니다 - 각 유스케이스는 별도로 고려되어야 합니다. 이 접근 방식은 [Vertical Slices](https://jimmybogard.com/vertical-slice-architecture) 아키텍처에 가깝고, 전체 애플리케이션 수준이 아닌 모듈 수준에서 적용됩니다.

일부는 도메인 중심 아키텍처와 수직 슬라이스가 상반된다고 말합니다. 이는 사실과는 거리가 멉니다 - 제 생각에, 이들은 서로를 완벽하게 보완합니다.

|![Module application architecture styles](/posts/development-contents/modular-monolith/ModulesArchitecture.jpg)|
|:--:|
|Module application architecture styles|
|모듈 애플리케이션 아키텍처 스타일|

### Module Data

각 모듈은 자체 상태를 가져야 하므로, 그 데이터는 비공개여야 합니다. 우리는 [공유 데이터베이스 패턴(Shared Database Pattern)](https://www.enterpriseintegrationpatterns.com/patterns/messaging/SharedDataBaseIntegration.html)을 사용하고 싶지 않습니다. 이는 모듈의 자율성과 모듈화를 달성하기 위해 필요한 핵심 속성입니다. 만약 모듈의 상태를 알고 싶거나 변경하고 싶다면 인터페이스를 통해 해야 합니다. 지름길은 없습니다.

그러나, 때때로 우리는 보고 목적으로 일부 데이터를 공유하고 싶을 수 있습니다. 이 경우, 별도의 [보고 데이터베이스(Reporting Database)](https://martinfowler.com/bliki/ReportingDatabase.html)를 사용하고, 모듈 데이터베이스에 대한 별도의 뷰를 특별한 통합 스키마 형태로 제공할 수 있습니다. [이것은 이러한 종류의 통합에만 해당됩니다]. 이 방식으로, 우리는 데이터베이스 수준에서 API 개념을 만들어냅니다. 이는 좋은 일입니다.

### Modules integration

이전 글에서 모듈 통합(modules integration)에 대해 자세히 썼습니다. 보시다시피, *모듈러 모노리스* 아키텍처 디자인은 통신의 2가지 형태를 가정합니다:

1. 이벤트를 통한 [비동기식] 통신 ([이벤트 주도 아키텍처(Event-Driven architecture)](https://en.wikipedia.org/wiki/Event-driven_architecture)). 각 모듈은 이벤트 버스를 통해 특정 이벤트를 보내거나 구독합니다. 이 이벤트 버스는 메모리 내 메커니즘이거나 프로세스 외부 컴포넌트일 수 있습니다. 필요에 따라 다릅니다.
1. 메모리 내 호출을 통한 [동기식] 통신. 여기서는 모듈과의 API 통신의 경우와 마찬가지로, 전통적인 접근법이나 *CQRS 스타일*(명령/질의)로 구현될 수 있습니다. 여기서 중요한 것은 소비자 측에서 [게이트](https://martinfowler.com/eaaCatalog/gateway.html)(어댑터)를 생성하고 공급자 측에서 [파사드](https://en.wikipedia.org/wiki/Facade_pattern)(포트와 그 구현체)를 생성함으로써 이러한 **통합이 명시적으로 이루어져야 한다**는 것입니다.

### Tests

시스템을 모듈화하고 싶다면 그것이 사소하지 않다는 의미입니다(또는 미래에는 그렇지 않을 것입니다.). 이것은 자동화된 테스트가 필수적임을 의미합니다. 그러나 어떤 유형의 테스트를 어느 정도 작성해야 하는지는 해당 시스템, 복잡성 수준, 통합 수, 그리고 다른 요소에 따라 다릅니다.

테스트는 이 글의 범위를 벗어나는 광범위한 주제로, 분명히 별도의 글을 필요로 합니다. 여기서는 고려해야 할 테스트를 강조하고 싶었습니다.

#### End to End tests

E2E 테스트는 전체 시스템을 테스트합니다. API에서 인프라로, 그리고 다시 API로. 이들은 종종 우리 시스템의 전체 조각을 검사하므로 테스트 코드 커버리지가 가장 큽니다.

그러나, 이들은 종종 시스템을 GUI와 함께 테스트하기도 합니다. 이로 인해, 이들은 가장 느리고, 취약하며, 유지 관리하기 어렵습니다.

#### Integration Tests

통합 테스트는 다양한 방식으로 이해되는 넓은 용어입니다. 제안된 아키텍처에서, 이들은 API 계층 없이 주어진 모듈 (또는 모듈 간 상호 작용)의 종합적인 테스트입니다. 이러한 유형의 통합 테스트는 모듈의 두 번째 소비자입니다(단지 또 다른 어댑터일 뿐입니다). API 계층이 매우 얇기 때문에, 이들은 사실상 전체 애플리케이션을 커버하며, 저수준 추상화 객체(JSON, HTTP)에 작동하지 않습니다.

#### Unit Tests

단위 테스트는 주로 *도메인 모델* - 비즈니스 로직을 테스트하는 데 사용됩니다. 모듈의 일부로 *도메인 중심* 아키텍처를 사용하기때문에, *도메인 모델*은 인프라로부터 분리되어 메모리에서 쉽게 테스트할 수 있습니다.

## Summary

보시다시피, 시스템을 모듈화하려면 적절한 설계의 규칙과 원칙을 따르는 규율이 필요합니다. 전체 시스템은 지속적으로 더 작은 단편으로 분해됩니다. 퍼즐의 모든 조각이 중요합니다. 이 아키텍처의 가장 중요한 속성들을 정리해봅시다:

- 다중 수준에서의 도메인 중심
- 잘 정의된 통합 지점 (인터페이스)
- 자체적으로 완성된, 캡슐화된 모듈
- 테스트 가능성 - 프레임워크와 인프라로부터 애플리케이션과 도메인 계층을 독립시킴
- 진화 가능성 - 새로운 모듈이나 어댑터를 추가하거나 유지 보수하는 것이 쉬움

시스템을 분산시킬 필요가 없고(대부분의 사람들이 필요로 하지 않음) 그리고 시스템이 비단순한 경우 *도메인 중심 설계*를 염두에 두고 *모듈러 모놀리틱*가 적합할 수 있습니다. 그러나, 모든 것은 프로젝트가 존재하는 상황에 따라 달라지므로 의식적으로 결정을 내려야 합니다.

## Additional Resources

Image credits: [Magnasoma](https://magnasoma.com/)
