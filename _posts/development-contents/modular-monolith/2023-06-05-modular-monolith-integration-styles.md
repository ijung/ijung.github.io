---
title: MODULAR MONOLITH - INTEGRATION STYLES (모듈러 모놀로스 - 통합 스타일)
author: June
date: 2023-06-05 10:00:00 +0900
categories: [개발 게시물, MODULAR MONOLITH]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Kamil Grzybek](https://www.kamilgrzybek.com/about)의 포스트 [MODULAR MONOLITH: INTEGRATION STYLES](https://www.kamilgrzybek.com/blog/posts/modular-monolith-integration-styles)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Introduction

더 큰 시스템 내의 모듈이나 애플리케이션은 100% 독립적으로 작동하지 않습니다. 비즈니스 가치를 제공하기 위해 개별 요소들은 어떻게든 서로 통합되어야 합니다. 여기에 [Thinking in Systems: A Primer](https://www.amazon.com/Thinking-Systems-Donella-H-Meadows/dp/1603580557)라는 책에서 Donella H. Meadows가 일반적으로 시스템 개념을 정의하는 인용구를 적어보겠습니다:

> A system is an interconnected set of elements that is coherently organized in a way that achieves something. If you look at that definition closely for a minute, you can see that a system must consist of three kinds of things: [elements], [interconnections], and a [function or purpose].
>> 시스템은 어떤 것을 달성하기 위해 일관성 있게 조직된 상호 연결된 요소 집합입니다. 이 정의를 잠시 자세히 살펴보면, 시스템은 세 가지 종류의 것들로 구성되어야 함을 알 수 있습니다: [요소], [상호 연결], 그리고 [기능 또는 목적].

시스템 통합 개념은 다음과 같이 정의됩니다 ([위키](https://en.wikipedia.org/wiki/System_integration)):

> …[process of linking together] different computing systems and software applications physically or functionally, to act as a [coordinated whole].
>> ...다른 컴퓨팅 시스템과 소프트웨어 애플리케이션을 물리적이거나 기능적으로 [연결하는 과정]을 통해 [조율된 전체] 역할을 합니다.

위의 정의에서 볼 수 있듯이, 목적을 충족하는 시스템을 제공하기 위해서는 요소를 통합하여 전체를 형성해야 합니다. 이 시리즈의 이전 글에서는, 우리 용어로 *모듈*이라 불리는 이러한 요소들의 특성에 대해 논의했습니다.

이 포스트에서는 누락된 부분인 *Modular Monolith* 아키텍처에서 모듈에 대한 *통합 스타일(Integration Styles)*에 대해 논의하고 싶습니다.

## Enterprise Integration Patterns book

이 게시물의 제목은 우연이 아닙니다. 이것은 Gregor Hohpe와 Bobby Wolf의 훌륭한 책 [Enterprise Integration Patterns](https://www.amazon.com/o/asin/0321200683)의 제2장과 완전히 같은 내용을 이야기 할 것 입니다. 이 책은 시스템 통합과 메시징에 대한 정보의 바이블로 간주됩니다. 이 글은 이 장에서 얻은 일부 지식을 모놀리틱 아키텍처와 모듈형 아키텍처에 적용합니다.

어쨌든, 통합 주제에 관심이 있는 모든 사람들을 책을 읽거나 [https://www.enterpriseintegrationpatterns.com/](https://www.enterpriseintegrationpatterns.com/) 사이트에서 온라인으로 사용할 수 있는 자료를 읽어 보시기 바랍니다.

## Integration Styles

### Integration Criteria

자연의 모든 것이 그러하듯이, 각 *통합 스타일*은 장단점이 있습니다. 따라서, 모든 스타일을 비교할 기준을 정의해야 합니다. 그런 다음, 그 기준에 따라 향후 통합 방법을 결정하게 될 것입니다.

다음과 같은 기준을 구분할 수 있습니다: 결합도, 복잡성, 데이터 적시성.

1. Coupling

    결합도(coupling)는 두 모듈이 서로 얼마나 의존적인지를 측정하는 척도입니다 ([위키](https://en.wikipedia.org/wiki/Coupling_(computer_programming))):

    > coupling is the [degree of interdependence] between software modules; a measure of how closely connected two routines or modules are; the [strength of the relationships] between modules.
    >> 결합도는 소프트웨어 모듈 간의 [상호 의존도]입니다; 두 루틴 또는 모듈이 얼마나 밀접하게 연결되어 있는지를 측정합니다.; 모듈 간의 [관계의 강도].

    이 시리즈의 이전 게시물을 읽으셨다면, **모듈식 디자인의 가장 중요한 특성 중 하나는 독립성**임을 이미 알고 계실 것입니다. 따라서 결합도가 통합 스타일에 관한 더 중요한 기준 중 하나라는 것을 쉽게 추측할 수 있을 것입니다.

1. Complexity

    *통합 스타일*을 평가하는 두 번째 기준은 그 **복잡성의 수준**입니다. 일부 통합 방법은 단순합니다 - 적은 작업을 필요로 하며, 이해하고 사용하기 쉽습니다. 그러나 다른 일부는 더 복잡하며, 더 많은 헌신, 지식, 그리고 규율이 필요합니다.

1. Data Timeliness

    마지막 기준은 [한 모듈이 일부 데이터를 공유하기로 결정한 시점과 다른 모듈들이 그 데이터를 가지게 되는 사이의 시간]입니다. 즉, 주어진 모듈에서 상태 변경이 발생한 직후, 관련된 나머지 모듈들이 이 변경 사항을 얼마나 빨리 고려하게 되는지를 의미합니다. 물론, 이 시간이 짧을수록 좋습니다.

이제 가장 중요한 모든 기준을 알았으니, 우리 모듈을 통합하는 방법으로 넘어가 보겠습니다. 4가지 *통합 스타일*: *파일 전송*, *공유 데이터베이스 데이터*, *직접 호출*, 그리고 *메시징*에 대해 논의해 보겠습니다.

### File Transfer

첫 번째 옵션은 **일반 파일**을 사용하여 모듈을 통합하는 것입니다. 이러한 파일은 소스 모듈에서 **내보내져서(exported)** 대상 모듈로 **가져와야(imported)** 합니다. 이것은 3가지 방법으로 이루어질 수 있습니다:

- 수동, 사용자가 수동으로 가져오기/내보내기를 하는 경우
- 자동, 파일이 시스템에 의해 자동으로 가져와지고 내보내지는 경우
- 하이브리드, 파일이 한 쪽에서는 자동으로 가져오기/내보내기되고 다른 쪽에서는 그 반대로 이루어지는 경우

|![Modular Monolith Integration Styles - File Transfer](/posts/development-contents/modular-monolith/Modular-Monolith-Integration-Styles-File-Transfer-1024x565.jpg)|
|:--:|
|Modular Monolith Integration Styles - File Transfer|
|모듈러 모놀리스 통합 스타일 - 파일 전송|

이 유형의 통합의 주요 작업 중 하나는 주어진 파일의 [형식을 결정(determine the format)]하는 것입니다. 중요한 것은, 이러한 방식으로 통합하는 두 모듈이 갖는 유일한 종속성입니다. 파일 시스템을 통해 전달되는 정말 큰 메시지로 시각화할 수 있습니다. 이런 이유로, 이 경우 [결합도는 매우 낮다(coupling is very low)]고 가정할 수 있습니다.

이 접근법의 복잡성 수준에 대해서는, 평균적으로 평가될 수 있습니다. 한편으로는, 특정 형식의 파일을 생성하는 것은 이 시대에는 어렵지 않습니다. 그러나 다른 한편으로는, 공유 리소스에 업로드하는 것, 파일 관리, 중복 처리 등은 더 복잡하고 시간이 많이 소요됩니다.

적시성의 관점에서 보면, **파일을 통한 모듈 통합은 느립니다**(수동 내보내기/가져오기는 말할 것도 없습니다). 대부분의 경우, 더 큰 배치에서 일정 시간 간격(소위 배치)으로 수행되며, 종종 밤에 이루어집니다. 이 때문에, 지연 시간은 하루, 일주일 혹은 그 이상이 될 수 있습니다.

솔직히 말해서, 저는 시스템 간에 파일 공유 통합을 많이 보았지만, 모놀리스에서는 한 번도 본 적이 없습니다 - 이해할 수 있는 부분입니다. 이 *통합 스타일*은 이 주제의 완전성을 위해 설명하였습니다. 모놀리스에서 가장 인기 있는 통합 방법은 *공유 데이터베이스 데이터(Shared Database Data)*입니다.

### Shared Database Data

EIP 책에서는 이 통합 방법을 *공유 데이터베이스(Shared Database)*라고 부르지만, 제 생각에는 이것이 정확한 이름은 아닌 것 같습니다. **데이터베이스를 공유한다는 것은 항상 데이터를 공유한다는 것을 의미하지는 않습니다**. 왜냐하면 모듈들은 데이터를 별도의 테이블에 저장할 수 있기 때문입니다(대부분은 데이터베이스 스키마를 통해 이루어집니다). 따라서 제 생각에는 *공유 데이터베이스 데이터(Shared Database Data)*가 더 적합한 용어입니다.

|![Modular Monolith Integration Styles - Shared Data](/posts/development-contents/modular-monolith/Modular-Monolith-Integration-Styles-Shared-Data-1024x724.jpg)|
|:--:|
|Modular Monolith Integration Styles - Shared Data|
|모듈러 모놀리스 통합 스타일 - 공유 데이터|

*공유 데이터베이스 데이터*에서 **모듈들은 데이터베이스 내의 특정 데이터 세트를 공유합니다**. 이런 식으로, 데이터는 항상 통합되어 있고 서로 일관성을 유지합니다. 왜냐하면 일반적으로 그들은 같은 데이터이기 때문입니다. 모듈 A가 테이블 X에 데이터를 쓰면, 모듈 B는 데이터베이스 트랜잭션이 완료된 직후에 데이터를 읽을 수 있습니다.

이러한 해결책의 복잡성 수준은 매우 낮습니다. 오늘날 모든 애플리케이션/모듈은 데이터베이스가 필요하므로, 이 접근법으로는 추가적인 것을 더할 필요가 없습니다.

첫눈에 이 해결책은 완벽해 보입니다. 그러나 가장 큰 단점은 **결합도가 매우 높다(very high coupling)**는 것입니다. 데이터를 공유함으로써, [모듈들은 서로의 상태]를 공유하게 되며, 이것이 그들을 결합시킵니다. 높은 결합도는 모듈에 대한 자율성이 없음을 의미합니다. 또한, 데이터베이스 구조나 심지어 데이터 자체에 대한 작은 변화도 다른 모듈을 무심코 고장내버릴 수 있습니다. 이것은 데이터베이스에 대한 모든 변경을 참조하고 조정하여야 함을 의미합니다. 이런 식으로 데이터베이스가 변경의 병목 현상이 됩니다. 전체 해결책은 더 이상 [진화형](https://www.thoughtworks.com/evolutionary-architecture)이 아닙니다.

공유 상태는 또 다른 중요한 단점을 가지고 있습니다 - 모든 모듈의 요구사항을 충족시킬 수 있는 **하나의 통합된 데이터 모델을 만드는 것이 매우 어렵거나 불가능**할 수 있습니다. 통합을 시도하는 것은 대부분 이해하고 개발하고 유지하기 어려운 매우 약한, 모호한 모델로 끝나게 됩니다.

결합도를 줄이면서도 데이터 적시성 수준을 유지하기 위해 *직접 호출(Direct Call)*을 사용할 수 있습니다.

### Direct Call

세 번째 옵션은 통합하려는 모듈의 **메소드를 직접 호출**하는 것입니다. 이 경우, 우리는 [캡슐화](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)) 메커니즘을 사용합니다. [모듈은 필요한 것만을 노출합니다]. 전체 동작은 메소드 안에 담겨 있습니다. 이렇게 하면, 우리의 모듈의 상태는 *공유 데이터베이스 데이터* 접근법의 경우처럼 외부에 노출되지 않습니다. 덕분에, 호출자는 외부에서 어떤 것도 망가뜨릴 수 없습니다.

|![Modular Monolith Integration Styles - Direct Call](/posts/development-contents/modular-monolith/Modular-Monolith-Integration-Styles-Direct-Call-1-1024x434.jpg)|
|:--:|
|Modular Monolith Integration Styles - Direct Call|
|모듈러 모놀리스 통합 스타일 - 직접 호출|

데이터를 공유하지 않는 것은 각 모듈이 자체 데이터 세트를 가지고 있다는 것을 의미합니다. 스키마로 나누어진 같은 데이터베이스일 수 있고, 각 모듈은 다른 기술로 만든 별도의 데이터베이스를 가질 수도 있습니다. [하나의 데이터베이스 시나리오에서는, 데이터를 실제로 격리시키는 것이 중요합니다]. 이는 별개의 모듈에서 테이블 간의 제약 없이 그리고 그들 사이의 트랜잭션 없이 이루어져야 함을 의미합니다.

호출자와 피호출자는 서로를 외부로 취급해야 합니다. 두 모듈은 다른 [언어](https://martinfowler.com/bliki/UbiquitousLanguage.html)를 사용하고 다른 개념을 모델링할 것입니다. 따라서 [Anti-Corruption Layer (ACL)](https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)를 적용해야 합니다. 호출자 측에서는 이것이 단지 [게이트웨이(gateway)](https://martinfowler.com/eaaCatalog/gateway.html)가 될 수 있고, 피호출자 측에서는 [퍼사드(facade)](https://en.wikipedia.org/wiki/Facade_pattern)가 될 수 있습니다. 덕분에, 모듈 캡슐화가 유지됩니다.

분산 시스템의 경우, 직접 호출은 [원격 프로시저 호출(Remote Procedure Invocation/Call (RPI/RPC))](https://en.wikipedia.org/wiki/Remote_procedure_call)로 알려져 있습니다. 불행히도, 이 기술은 *마이크로서비스* 아키텍처에서 매우 자주 사용되며, 소위 [분산형 모놀리스(Distributed Monolith)](https://www.simplethread.com/youre-not-actually-building-microservices/) 안티패턴 아키텍처로 이어질 수 있습니다. 호출이 항상 동기적이므로, 우리는 시간적 결합을 처리하게 됩니다. 호출자와 피호출자는 동시에 사용 가능해야 합니다. 모놀리스의 경우, 이것은 그것의 본질이므로 문제가 되지 않지만, 마이크로서비스의 경우는 훨씬 더 나쁩니다 - 자율적인 개발과 배포와 같은 아키텍처 품질 속성을 감소시킵니다. 이에 대한 자세한 내용은 이 시리즈의 다른 기사를 읽어보십시오.

*직접 호출 통합 스타일*은 우리의 모듈을 통합하는 데 있어 매우 좋은 선택처럼 보이지만, 일부 단점도 있습니다. 첫째, 호출이 동기적이므로 호출자는 결과를 기다려야 합니다. 둘째, [호출하는 모듈은 호출되는 모듈에 대해 알아야 하며], **직접적인 종속성**을 가져야 합니다. 게다가, 그것이 무엇을 하고자 하는지 알아야 합니다. 결합도는 *공유 데이터베이스 데이터*보다 낮지만 여전히 존재합니다. 이런 단점들을 피하고 싶다면, 마지막 통합 스타일인 *메시징(Messaging)*을 사용할 수 있습니다.

### Messaging

*파일 전송* 통합 스타일은 큰 장점이 있습니다 - 모듈 간의 의존성을 생성하지 않습니다. 그러나, 큰 단점이 있습니다 - 대부분의 경우 데이터의 실시간성이 불만족스럽습니다. 메시징은 이런 단점이 없습니다. 데이터의 실시간성은 *직접 호출*만큼 좋지는 않습니다 왜냐하면 비동기 통신이기 때문입니다. 하지만, 대부분의 경우에 매우 좋고, 수용 가능 정도의 실시간성을 가집니다.

|![Integration Styles - Messaging (in memory)](/posts/development-contents/modular-monolith/Modular-Monolith-Integration-Styles-Messaging-in-memory-1024x485.jpg)|
|:--:|
|Integration Styles - Messaging (in memory)|
|통합 스타일 - 메시징(메모리 내)|

|![Integration Styles - Messaging (separate process)](/posts/development-contents/modular-monolith/Modular-Monolith-Integration-Styles-Messaging-separate-process-1024x769.jpg)
|:--:|
|Integration Styles - Messaging (separate process)|
|통합 스타일 - 메시징(별도의 프로세스)|

[이벤트 기반 아키텍처(Event-Driven Architecture)](https://en.wikipedia.org/wiki/Event-driven_architecture)의 구현을 위한 메시징의 적절한 사용은 모듈 간의 의존성을 생성하지 않습니다. 모듈들은 이벤트를 통해 통합됩니다. 그러나 이것들은 [도메인 이벤트(Domain Event)](https://martinfowler.com/eaaDev/DomainEvent.html)가 아닙니다 왜냐하면 도메인 이벤트는 로컬이며 주어진 [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html) 안에 캡슐화되어야 하기 때문입니다. 통합 이벤트는 [팻 이벤트(Fat Ebents)](https://verraes.net/2019/05/patterns-for-decoupling-distsys-fat-event/)를 피하기 위해 필요한 만큼만 포함합니다. [통합 이벤트는 가능한 한 작아야 합니다], 왜냐하면 **그것들은 주어진 모듈에 의해 제공되는 계약의 일부**이기 때문입니다. 알다시피, [계약이 작을수록 그것은 더 안정적이고] -> 다른 모듈이 변경해야 하는 빈도가 더 적습니다.

또한, 비동기 처리는 [최종 일관성(Eventual Consistency)](https://en.wikipedia.org/wiki/Eventual_consistency)을 가집니다. 그리고 성능 이점을 지원하고, 더 나은 확장성을 가지며, 더 신뢰할 수 있습니다.

*메시징*의 단점은 무엇일까요? 첫째, 비동기성의 특성으로 인해, 위에서 설명한 바와 같이 **전체 시스템의 상태는 결국 일치하게 됩니다**. 그래서 모듈의 경계를 잘 정의하는 것이 매우 중요합니다. 이것은 *마이크로서비스* 아키텍처에서 거의 중요하다고 할 수 있지만, *모듈러 모놀리스* 아키텍처의 장점은 이러한 경계를 변경하는 것이 훨씬 쉽다는 것입니다.

*메시징*의 두 번째 단점은 그것이 [더 복잡하다]는 것입니다. 비동기 처리와 *이벤트 기반 아키텍처(Event-Driven Architecture)*를 제공하기 위해서는, 우리는 어떤 종류의 이벤트 버스가 필요합니다. 이것은 메모리 내 브로커일 수도 있고 별도의 컴포넌트(예: [RabbitMQ](https://www.rabbitmq.com/))일 수도 있습니다. 또한, 우리는 내부 처리를 위한 작업 처리 메커니즘도 필요합니다 - 아웃박스, 인박스, 내부 명령 메시지. 이 인프라스트럭처 코드 중 일부를 작성해야 합니다. 다행히도, 이것은 일반적인 문제입니다 - 우리는 이것을 한 번만 하면 되며, 이를 지원하는 많은 라이브러리와 프레임워크가 있습니다.

### Comparison

아래에서는 결합, 데이터 적시성 및 복잡성의 3가지 기준을 고려하여 3가지 *통합 스타일*을 모두 비교합니다.

|![Comparison - Coupling vs Data Timeliness](/posts/development-contents/modular-monolith/Comparison-Coupling-vs-Data-Timeliness-1024x791.jpg)|
|:--:|
|Comparison - Coupling vs Data Timeliness|
|비교 - 결합 대 데이터 적시성|

이 다이어그램들로부터 우리는 무엇을 추론할 수 있을까요? 먼저, 완벽한 스타일은 없다는 것입니다. 다행히도, 몇 가지 휴리스틱을 볼 수 있습니다:

1. 시스템이 매우 매우 단순하다면([핵심 복잡성](https://wiki.c2.com/?EssentialComplexity)이 낮다면) 그리고 모듈성에 대해 신경 쓰지 않는다면: 단순함을 선택하고 *Shared Database Data* 스타일을 사용하십시오.
1. 시스템이 복잡하다면(핵심 복잡성이 높다면) **반드시 모듈성을 신경 써야 합니다**. 선택사항은 다음과 같습니다:

    - 모듈 간의 자율성이 가장 높아야 하고, 모듈 간의 최종 일관성이 허용되는 경우 *메시징*을 선택하십시오.
    - 성능, 신뢰성, 확장성에 대해 매우 신경 쓰는 경우 *메시징*을 선택하십시오.
    - 강력한 일관성이 필요하고, 모듈의 최대 수준의 자율성이 필요하지 않거나 *데이터 적시성*이 주요 요소인 경우 *Direct Call*을 선택하십시오.

물론, **여러 스타일들을 함께 사용**하는 것도 가능합니다. 대부분의 경우, 모듈화 아키텍처에 대해 이야기할 때, *Direct Call*과 *Messaging*을 함께 사용하는 것이 가장 좋습니다. 필요에 따라 일부 모듈은 동기적으로 통신하고 일부는 비동기적으로 통신할 수 있습니다.

## Summary

처음에 언급했듯이, 어떤 모듈, 컴포넌트, 시스템도 완전히 고립되어 존재하지 않습니다. "나누고 정복하라(Divide and Conquer)"라는 원칙을 따라야 하므로 우리는 우리의 솔루션을 더 작은 부분으로 나눕니다. 하지만 결국 - 이러한 부분들을 함께 통합하여 시스템을 만들어야 합니다.

4가지 스타일을 다시 한번 요약해 봅시다:

- *파일 전송(File transfer)* - 낮은 결합도를 제공하지만 거의 모든 경우에서 데이터 적시성이 불충분합니다. 그렇기 때문에 모놀리스에서는 비현실적입니다.
- *공유 데이터베이스 데이터(Shared Database Data)* - 가장 간단하고 빠르지만, 모듈들을 결합시킵니다.
- *직접 호출(Direct Call)* - 공유 데이터베이스 데이터보다 낮은 결합도를 제공하며, 모듈을 캡슐화하고 상대적으로 단순합니다.
- *메시징(Messaging)* - 가장 낮은 결합도, 모듈화, 자율성을 보장하지만 복잡성에 따른 비용이 높습니다.

어떤 *통합 스타일*을 선택하여야 할까요? 모든 것은 평소처럼 "상황에 따라 다르다"입니다. 그러나 어느 정도 그것이 어떤 것에 따라 다른지를 설명하는 데 성공했다고 생각합니다. 다시 한번이야기 하지만, [만능 해결책은 없습니다(No silver bullet)](https://en.wikipedia.org/wiki/No_Silver_Bullet).
