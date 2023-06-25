---
title: 모듈러 모놀리스 아키텍처(modular monolith architecture)
author: June
date: 2023-05-25 21:39:00 +0900
categories: [개발 아키텍처, 모듈러 모놀리스 아키텍처]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 모듈러 모놀리스 아키텍처(modular monolith architecture)란?

모듈러 모놀리스 아키텍처는 소프트웨어 아키텍처의 진화 과정에서 나타난 아키텍처입니다.

기존의 전통적인 모놀리스 아키텍처는 낮은 유연성으로 인해 복잡도가 증가함에 따라 확장이 어려워지고 생산성이 낮아지는 등 많은 문제점들이 있었습니다. 이를 해결하기 위해 서비스 오리엔티드 아키텍처가 부상하였고, 이러한 서비스 오리엔티드 아키텍처의 아이디어를 더욱 발전시킨  마이크로서비스 아키텍처가 대세로 자리잡았습니다.

|![모놀리스 아키텍쳐는 복잡도가 올라감에 따라 생산성이 급격히 감소합니다.](/posts/development-architectures/productivity.png)|
|:--:|
|[모놀리스 아키텍쳐는 복잡도가 올라감에 따라 생산성이 급격히 감소합니다.](https://martinfowler.com/bliki/MicroservicePremium.html)|

마이크로서비스를 도입하며 전통적인 모놀리스 아키텍처의 많은 단점들을 극복할 수 있었습니다. 하지만, 복잡한 인프라 구성과 많은 통신 오버헤드, 서비스 분리로 인한 테스트와 디버깅의 어려움 등 기본적으로 운영의 복잡성과 비용이 높은 새로운 문제점이 부각되었습니다.

모듈러 모놀리스 아키텍처는 이 두 가지 아키텍처의 사이에서 균형을 맞춰 장점을 결합하고 단점을 상쇄하여 복잡한 시스템을 더 쉽게 관리하고자 하는 목적으로 나온 아키텍처입니다.

모듈러 모놀리스 아키텍처는 2018년 GOTO Conference에서의 Simon Brown이 [Modular Monoliths 발표](https://youtu.be/5OjqD-ow8GE)와 2019년 Kamil Grzybek이 블로그에 [Modular Monolith 시리즈를 포스팅](https://www.kamilgrzybek.com/blog/categories/modular-monolith)을 통해하면서 그 이름과 개념이 자리 잡았습니다. 그리고 2020년 우아한형제들의 박용권님이 배민에서 [모듈러 모놀리스 아키텍처를 도입한 과정에 대한 세미나](https://youtu.be/SrQeIz3gXZg)를 진행하는 등 점점 그 이름이 알려지고 있고, 사용 사례 또한 늘고 있습니다.

### Monolith

[Monolith(Wikipedia)](https://en.wikipedia.org/wiki/Monolith)
> A monolith is a geological feature consisting of a single massive stone or rock, such as some mountains.
>> 모놀리스는 일부 산과 같이 하나의 거대한 돌 또는 바위로 구성된 지질학적 특징

[Monolithic Architechture(Wikipedia)](https://en.wikipedia.org/wiki/Monolithic_architecture)
> Monolithic architecture describes buildings which are carved, cast or excavated from a **single piece** of material, historically from rock.
>> 모놀리식 아키텍처는 역사적으로 돌에서 채굴, 주조, 또는 조각된 **단일 조각**의 재료로 만들어진 건물

이 정의를 컴퓨터 과학에 대입하면 컴퓨터 과학에서의 Monolithic Architecture의 정의를 유추할 수 있습니다. 컴퓨터 과학의 관점에서 볼 때, 건물은 시스템이고 재료는 우리의 실행 코드입니다. 따라서 *Monolith Architecture*란, **하나의 실행 코드(배포 단위)로 구성된 시스템**을 의미합니다.

[Monolith System(Wikipedia)](https://en.wikipedia.org/wiki/Monolithic_system)
> A monolithic system is a system that is integrated into one whole, analagous to a monolith.
> In application software, software is called "monolithic" if it has a monolithic architecture, in which functionally distinguishable aspects (for example data input and output, data processing, error handling, and the user interface) are all interwoven, rather than containing architecturally separate components.
>> 모놀리식 시스템은 전체가 하나로 통합된 시스템으로, 모놀리스와 유사합니다.
>> 응용 소프트웨어에서는 기능적으로 구분 가능한 부분들(예를 들어 데이터 입력과 출력, 데이터 처리, 에러 처리, 그리고 사용자 인터페이스)이 모두 서로 뒤섞여 있는 모놀리식 아키텍처를 가지면 '모놀리식'이라고 부른다. 이는 아키텍처적으로 분리된 컴포넌트를 포함하는 것이 아니라 모든 것이 서로 뒤섞여 있다는 것을 의미합니다.

이 정의에 따르면 모놀리스 시스템은 크게 2가지 특징을 가집니다. 첫 번째 특징은 모놀리스 시스템은 전체가 하나로 통합된 시스템이라는 점입니다. 모놀리스 시스템은 모놀리스 아키텍처가 시스템의 모든 부분이 하나의 배포(실행) 단위를 형성한다고 규정합니다. 두 번째 특징은 모든 것이 서로 뒤섞여 있다는 점입니다. 이러한 특징은 모놀리스 시스템은 모듈성(modularity)이 낮다고 가정하는 것이며, 모놀로식 아키텍처를 매우 부정적으로 특징짓습니다.
모놀로식 시스템의 첫 번째 특징은 다른 모놀로식과 관련된 정의와 일맥상통합니다. 하지만, 2번째 특징은 다른 모놀로식에 대한 정의에도 나타나 있지 않습니다. 즉, 모놀로식 시스템은 모든 것이 서로 뒤섞여 있을 수도 있지만 **반드시 그럴 필요는 없습니다**. 이러한 점은 모노리스의 궁극적인 속성은 아닙니다.

요약하자면, *Monolith System*은 **정확히 하나의 배포 단위가 있는 시스템**에 지나지 않습니다. 그 이상도 이하도 아닙니다.

### Modularization

[Modular(Cambridge Dictionalry)](https://dictionary.cambridge.org/dictionary/english/modular)
> Consisting of **separate parts** that, when combined, form a complete whole/made from a set of separate parts that can be **joined together to form a larger object**
>> 결합될 때 완전한 전체를 형성하는 **개별 부분**으로 구성되어 있지만, 이들이 결합될 때 완전한 전체를 형성한다. 분리된 여러 부분들로 구성된 집합으로부터 **더 큰 객체를 형성하기 위해 결합**할 수 있는 것

[Modularization(Cambridge Dictionalry)](https://dictionary.cambridge.org/dictionary/english/modularization)
> The design or production of something in separate sections
>> 어떤 것을 분리된 섹션으로 디자인하거나 생산하는 것

[Modular programming(Wikipidia)](https://en.wikipedia.org/wiki/Modular_programming)
> Modular programming is a software design technique that emphasizes separating the functionality of a program into independent, interchangeable modules, such that each contains everything necessary to execute only one aspect of the desired functionality. A module interface expresses the elements that are provided and required by the module. The elements defined in the interface are detectable by other modules. The implementation contains the working code that corresponds to the elements declared in the interface.
>> 모듈형 프로그래밍은 프로그램의 기능을 독립적이며 상호 교환 가능한 모듈로 분리하도록 강조하는 소프트웨어 디자인 기법입니다. 이렇게 하면 각 모듈은 원하는 기능의 일부를 실행하는 데 필요한 모든 것을 포함하게 됩니다. 모듈 인터페이스는 모듈이 제공하고 필요로 하는 요소를 표현합니다. 인터페이스에서 정의된 요소는 다른 모듈에서 감지할 수 있습니다. 구현체는 인터페이스에 선언된 요소에 대응하는 작동 코드를 포함합니다.

즉, Modular programming에서 모듈의 특징은 다음과 같습니다.

- a) 독립적이고 상호 교환 가능해야 하며
- b) 원하는 기능을 제공하는 데 필요한 모든 것이 있어야 하며
- c) 정의된 인터페이스가 있어야 합니다.

### Modular Monolith Architecture

즉, 모듈러 모놀리스 아키텍처란 **독립적이며 상호 교환 가능한 모듈들의 집합으로 구성된 하나의 배포 단위를 가지는 시스템**입니다.

## Modular Monolith Architecture의 구조와 특징

모듈러 모놀리스 아키텍처는 전통적인 모놀리스 아키텍처의 통신의 단순성과 마이크로서비스 아키텍처의 컴포넌트화의 유연성를 결합하여 서비스 확장시 복잡도 증가를 줄이면서 인프라와 서비스간 비동기 통신 등에 대한 관리 부담을 줄일 수 있도록 하는 아키텍처입니다.

|![모듈러 모놀리스 아키텍처 출생의 비밀(?)](/posts/development-architectures/modular_monolith_pandafusion_block.png)|
|:--:|
|[모듈러 모놀리스 아키텍처 출생의 비밀(?)](https://playtech.ee/blog/case-study-ditching-microservices-in-favor-of-a-modular-monolith)|

모듈러 모놀리스 아키텍처는 전통적인 모놀리스 아키텍처의 단순함과 마이크로서비스 아키텍처의 모듈화된 개발 사이의 균형을 찾는 아키텍처입니다.

### 구조(모듈화)

모듈러 모놀리스 아키텍처는 모듈들의 집합으로 구성되어 있습니다. 전통적인 모놀리스 아키텍처는 코드를 기능적 경계와 의존성을 중심으로 계층화 하는데 중점을 둡니다. 반면, 모듈러 모놀리스 아키텍처는 코드를 개별 기능 모듈로 분할하여 경계 맥락을 설정합니다. 각 모듈은 다른 모듈에 프로그래밍 인터페이스 정의만을 노출합니다.

|![계층 구조로 모듈화](/posts/development-architectures/-35-2048.webp)|
|:--:|
|[계층 구조로 모듈화](https://www.slideshare.net/arawnkr/ss-224478403)|

|![관심사에 따른 모듈화](/posts/development-architectures/-36-2048.webp)|
|:--:|
|[관심사에 따른 모듈화](https://www.slideshare.net/arawnkr/ss-224478403)|

|![전통적인 모놀리스 아키텍처와 마이크로 서비스 아키텍처](/posts/development-architectures/modular_monolith_1.png)|
|:--:|
|[전통적인 모놀리스 아키텍처와 마이크로 서비스 아키텍처](https://playtech.ee/blog/case-study-ditching-microservices-in-favor-of-a-modular-monolith)|

|![전통적인 모놀리스 아키텍처와 모듈러 모놀리스 아키텍처](/posts/development-architectures/modular_monolith_2_centered.png)|
|:--:|
|[전통적인 모놀리스 아키텍처와 모듈러 모놀리스 아키텍처](https://playtech.ee/blog/case-study-ditching-microservices-in-favor-of-a-modular-monolith)|

모듈러 모놀리스 아키텍처의 장점 중 하나는 로직의 캡슐화가 높은 재사용성을 가능하게 하고, 데이터가 일관되게 유지되며 통신 패턴이 단순하다는 것입니다. 모듈러 모놀리스 아키텍처는 로직의 캡슐화와 높은 재사용성 등의 마이크로서비스 아키텍처의 장점을 수용하면서도 수십 또는 수백 개의 마이크로서비스를 관리하는 것보다 기본 인프라 복잡성과 운영 비용을 낮아 서비스 관리를 더욱 쉽게 해 줍니다.

|![모듈러 모놀리스 아키텍처는 낮은 인프라 복잡성과 높은 모듈성을 가집니다.](/posts/development-architectures/msa_soa_tma_mma.png)|
|:--:|
|[모듈러 모놀리스 아키텍처는 낮은 인프라 복잡성과 높은 모듈성을 가집니다.](https://files.gotocon.com/uploads/slides/conference_12/515/original/gotoberlin2018-modular-monoliths.pdf)|

### 특징

- 모듈화: 모듈러 모놀리스 아키텍처는 시스템을 잘 정의된 모듈로 나눕니다. 각 모듈은 특정 기능을 수행하며, 서로간에는 최소한의 의존성을 유지합니다. 이로 인해 각 모듈은 독립적으로 개발, 테스트 될 수 있습니다.
- 통합된 코드베이스: 모놀리스 시스템과 마찬가지로, 모듈러 모놀리스 아키텍처는 단일 코드베이스를 유지합니다. 이는 코드 관리를 단순화하고 시스템 간 상호작용의 복잡성을 줄입니다.
- 동일한 프로세스 내에서 실행: 모듈러 모놀리스 아키텍처의 모든 모듈은 동일한 프로세스 내에서 실행됩니다. 이는 마이크로서비스와는 대조적으로, 네트워크 통신을 통한 오버헤드를 줄이고, 데이터 일관성을 유지하는데 도움을 줍니다.
- 영역 주도 설계(Domain-Driven Design, DDD): 모듈은 일반적으로 DDD 원칙에 따라 설계되며, 이는 각 모듈이 비즈니스 도메인 내의 특정 부분을 나타내도록 합니다.

## Modular Monolith Architecture 구조의 예시와 유용한 도구

### Modular Monolith Architecture 구조의 예시1: building-modular-monoliths-using-spring

|![모듈 구조](/posts/development-architectures/building-modular-monoliths-using-spring.png)|
|:--:|
|[모듈 구조](https://github.com/arawn/building-modular-monoliths-using-spring)|

|![모듈간 종속성](/posts/development-architectures/-45-2048.webp)|
|:--:|
|[모듈간 종속성](https://www.slideshare.net/arawnkr/ss-224478403)|

### Modular Monolith Architecture 구조의 예시2: modular-monolith-with-ddd

|![모듈 구조](/posts/development-architectures/VSSolution.png)|
|:--:|
|[모듈 구조](https://github.com/kgrzybek/modular-monolith-with-ddd)|

|![모듈간 종속성(High Level View)](/posts/development-architectures/Architecture_high_level.png)|
|:--:|
|[모듈간 종속성(High Level View)](https://github.com/kgrzybek/modular-monolith-with-ddd)|

|![모듈간 종속성(Module Level View)](/posts/development-architectures/Module_level_diagram.png)|
|:--:|
|[모듈간 종속성(Module Level View)](https://github.com/kgrzybek/modular-monolith-with-ddd)|

### 유용한 도구: Spring Modulith

스프링 프로젝트에서는 복잡한 애플리케이션을 더 작은, 독립적인 부분으로 모듈화하는 작업을 돕는 Spring Modulith 프레임워크를 제공하고 있습니다.
Spring Modulith는 Spring Data 프로젝트의 리드인 Oliver Drotbohm가 모듈화의 아이디어를 발전시켜 제작한 Moduliths 프레임워크가 스프링 프로젝트에 편입되면서 이름이 바뀌었습니다.
이 프레임워크는 각 모듈 간의 상호작용을 정의하는 역할을 하며, 개발자가 논리적 애플리케이션 모듈을 코드로 표현하고, 도메인 중심으로 잘 구조화된 스프링 부트 애플리케이션을 구축할 수 있도록 도와줍니다. 또한, 애플리케이션 모듈화를 위한 특별한 도구를 제공하여 모듈 구성 테스트와 모듈별 통합 테스트를 지원합니다. 추가로, 모듈 수준에서 애플리케이션의 동작을 관찰하여 C4 Model이나 UML과 같은 문서를 자동으로 생성합니다. 이런 기능은 개발자가 자신의 코드를 더 잘 이해하게 하고, 작업 중인 애플리케이션의 전체 구조를 시각화하는데 도움을 줍니다.

## Modular Monolith Architecture의 장점과 단점

모듈러 모놀리스 아키텍처는 모놀리스 아키텍처와 마이크로서비스 아키텍처의 많은 장점을 계승하고 있습니다. 하지만, 마이크로서비스 아키텍처의 회복성이나 기술 이기종성 등과 같은 모든 장점을 수용하지는 않습니다.

### 장점

- 단순성: 모듈러 모놀리스 아키텍처는 마이크로서비스에 비해 구현이 더 단순합니다. 이는 네트워크 통신, 데이터 일관성, 서비스 간의 동기화 등에 대한 복잡성을 피할 수 있습니다.
- 유지 보수성: 모듈화는 코드베이스를 더 쉽게 이해하고 유지 관리할 수 있게 합니다. 각 모듈은 독립적으로 개발, 테스트 될 수 있으며, 이는 복잡한 시스템을 관리하는 데 도움이 됩니다.
- 프로세스 내 통신: 모듈 간 통신이 프로세스 내에서 발생하기 때문에, 네트워크 지연이나 오버헤드 없이 빠른 통신이 가능합니다.
- 데이터 일관성: 모듈이 하나의 데이터베이스를 공유할 수 있으므로, 데이터 일관성을 유지하는 것이 비교적 쉽습니다. 마이크로서비스에서는 각 서비스가 자체 데이터를 관리하므로, 이러한 일관성을 유지하는 것이 더 어렵습니다.
- 점진적 마이크로서비스로의 전환: 모듈러 모놀리스 아키텍처는 마이크로서비스로의 점진적 전환을 가능하게 합니다. 이는 특정 모듈이 시스템의 나머지 부분과의 결합도를 최소화하고, 독립적으로 확장 가능하도록 하는 데 도움이 됩니다.

### 단점

- 모듈 간 의존성: 모듈 간의 의존성이 증가하면 전체 시스템의 유연성과 확장성이 제한될 수 있습니다. 이러한 의존성은 아키텍처를 더 복잡하게 만들고, 변경이나 유지 보수를 어렵게 할 수 있습니다.
- 한계된 확장성: 모든 모듈이 동일한 프로세스 내에서 실행되므로, 특정 모듈의 확장이 전체 시스템의 리소스를 제한할 수 있습니다. 마이크로서비스 아키텍처에서는 각 서비스를 개별적으로 확장할 수 있어 이런 문제를 더 잘 처리할 수 있습니다.
- 모듈 간 영향력: 하나의 모듈에서 문제가 발생하면, 동일한 프로세스 내에서 실행되는 다른 모듈에 영향을 줄 수 있습니다. 이는 전체 시스템의 안정성을 위협할 수 있습니다.
- 데이터 일관성 관리: 모듈간에 데이터를 공유하면 데이터 일관성을 유지하는 것이 더 쉬울 수 있지만, 동시에 데이터 액세스와 수정에 대한 관리와 보안 문제도 발생할 수 있습니다.
- 아키텍처 복잡성: 모듈을 잘 정의하고, 모듈 간의 경계를 관리하며, 의존성을 최소화하는 것은 복잡한 작업일 수 있습니다. 이는 추가적인 개발 및 유지 보수 노력을 필요로 합니다.

## 결론

모듈러 모놀리스 아키텍처는 전통적인 모놀리스 아키텍처의 단순함과 마이크로서비스 아키텍처의 유연성을 균형있게 수용합니다. 이를 통해, 모놀리스 아키텍처의 복잡성 증가에 따른 생성성 감소와 마이크로서비스 아키텍처의 인프라 복잡성 및 운영 비용과 같은 단점을 줄이려고 합니다.

다르게 표현하면, 모듈러 모놀리스 아키텍처는 전통적인 모놀리스 아키텍처와 마이크로서비스 아키텍처 사이에 위치하며, 각각의 장점을 취하고 단점을 최소화하려 합니다. 하지만, 모든 것이 그렇듯이 트레이드오프는 존재합니다.

모듈러 모놀리스 아키텍처의 적용과 그 장점을 최대화하기 위해 다음과 같은 고려사항들이 있습니다:

- 서비스의 규모
- 비즈니스의 복잡도
- 팀 크기
- 팀 문화

이러한 요소들이 모듈러 모놀리스 아키텍처의 적용에 적합한지에 대해 고민하고 결정하는 것이 중요합니다.

## Reference

- [MODULAR MONOLITH](https://www.kamilgrzybek.com/blog/categories/modular-monolith)
- [Modular Monoliths • Simon Brown • GOTO 2018(Video)](https://youtu.be/5OjqD-ow8GE)
- [Modular Monoliths • Simon Brown • GOTO 2018(Document)](https://files.gotocon.com/uploads/slides/conference_12/515/original/gotoberlin2018-modular-monoliths.pdf)
- [우아한 모노리스(Video)](https://youtu.be/SrQeIz3gXZg)
- [우아한 모노리스(Document)](https://www.slideshare.net/arawnkr/ss-224478403)
- [우아한 모노리스(Code: building-modular-monoliths-using-spring)](https://github.com/arawn/building-modular-monoliths-using-spring)
- [Case Study: Ditching Microservices in Favor of a Modular Monolith](https://playtech.ee/blog/case-study-ditching-microservices-in-favor-of-a-modular-monolith)
- [Modular Monolithic vs. Microservices](https://www.fullstacklabs.co/blog/modular-monolithic-vs-microservices)
- [spring Modulith](https://github.com/spring-projects/spring-modulith)
- [modular-monolith-with-ddd](https://github.com/kgrzybek/modular-monolith-with-ddd)

## Attachments
