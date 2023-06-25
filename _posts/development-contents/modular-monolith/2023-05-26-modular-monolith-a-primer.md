---
title: MODULAR MONOLITH - A PRIMER (모듈러 모놀로스 - 입문서)
author: June
date: 2023-05-25 21:39:00 +0900
categories: [개발 게시물, MODULAR MONOLITH]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Kamil Grzybek](https://www.kamilgrzybek.com/about)의 포스트 [MODULAR MONOLITH: A PRIMER](https://www.kamilgrzybek.com/blog/posts/modular-monolith-primer)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Introduction

마이크로 서비스 아키텍처의 인기가 높아진 지 수년이 지났으며 여전히 시스템 아키텍처의 맥락에서 논의되는 주요 주제 중 하나입니다. 클라우드 솔루션, 컨테이너화 및 분산 시스템(예: Kubernetes)의 개발 및 유지 관리를 지원하는 고급 도구의 인기는 이러한 현상에 훨씬 더 도움이 됩니다.

커뮤니티, 회사 및 프로그래머와의 대화 중에 무슨 일이 일어나고 있는지 관찰하면 대부분의 새 프로젝트가 마이크로 서비스 아키텍처를 사용하여 구현된다는 결론을 내릴 수 있습니다. 또한 일부 레거시 시스템도 이 접근 방식으로 이동하고 있습니다.

게시물의 주제는 *Modular Monolith*이고 나는 마이크로 서비스에 대해 이야기합니다. 즉, IT 산업으로서 우리는 마이크로 서비스 아키텍처를 채택하는 잘못된 출발을 했다고 생각하기 때문입니다. **아키텍처 핵심 요인에 초점을 맞추는** 대신, 우리는 마이크로서비스가 모놀리식 애플리케이션에 존재하는 모든 악에 대한 약이라고 믿었습니다. 둘 이상의 배포 단위로 구성된 시스템 개발에 참여한 경우 그렇지 않다는 것을 이미 알고 있습니다. **각 아키텍처에는 장단점이 있으며** 마이크로 서비스도 예외는 아닙니다. 그들은 그 대가로 다른 문제를 생성하여 일부 문제를 해결합니다.

나는 여러 가지 이유로, 이 항목을 통해 Modular Monolith의 아키텍처에 대한 일련의 기사를 시작하고 싶습니다.

우선, 모놀리식 아키텍처에서 고급 시스템을 만들 수 없다는 신화를 반박하고 싶습니다. 둘째, 나는 이 아키텍처의 정의와 형태에 대한 의구심을 없애고 싶습니다 - 많은 사람들이 그것을 다르게 해석합니다. 셋째, 필자는 이 일련의 게시물을 몇 달 전에 GitHub에서 공유했으며 매우 호평을 받은 [Modular Monolith with DDD](https://github.com/kgrzybek/modular-monolith-with-ddd) 아키텍처 구현에 대한 확장 및 추가 기능으로 취급합니다. 이 구현은 몇 달 전에 GitHub에서 공유했고 매우 호평을 받았습니다(게시 후 한 달에 별 1,000개).

이 소개 게시물에서는 Modular Monolith 아키텍처의 정의에 중점을 둘 것입니다.

## What is Modular Monolith?

저는 기술 및 비즈니스 문제, 특히 아키텍처와 관련하여 이야기하거나 글을 쓸 때 항상 정확하려고 노력합니다. 명확하고 일관된 메시지가 매우 중요하다고 생각합니다. 그렇기 때문에 Modular Monolith의 아키텍처가 나에게 의미하는 바는 무엇이며 그것을 인식하는 방법을 명확하게 정의하고 싶습니다.

더 간단한 개념부터 시작하겠습니다. 모놀리스란 무엇입니까?

### Monolith

[Wikipedia](https://en.wikipedia.org/wiki/Monolithic_architecture)는 다음과 같이 컴퓨터 과학이 아닌 건물 건설 측면에서 "모놀리식 아키텍처"를 설명합니다.

> Monolithic architecture describes buildings which are carved, cast or excavated from a "single piece" of material, historically from rock.
>> 모노리식 구조는 건물이 "단일 재료", 역사적으로는 암석에서 조각, 주조, 또는 채굴되는 구조를 설명합니다.

컴퓨터 과학의 관점에서 볼 때, 건물은 시스템이고 재료는 우리의 실행 코드입니다. 따라서 *Monolith Architecture*에서 우리 **시스템은 정확히 하나의 실행 코드로 구성**됩니다.

두 가지 기술적 정의를 살펴보겠습니다: [Monolith System](https://en.wikipedia.org/wiki/Monolithic_system)에 대한 첫 번째 정의:

> A monolithic system is a system that is integrated into one whole, analagous to a monolith. The phrase can have slightly different meanings in the contexts of computer software and hardware.
>> 소프트웨어 시스템은 기능적으로 구별 가능한 측면(예: 데이터 입력 및 출력, 데이터 처리, 오류 처리 및 사용자 인터페이스)이 구조적으로 분리된 구성 요소를 포함하지 않고 모두 상호 결합되어 있는 모놀리식 아키텍처를 가지고 있는 경우 '모놀리식'이라고 합니다.

[Monolith Architecture](https://www.techtarget.com/whatis/definition/monolithic-architecture)에 대한 두 번째 정의:

> 모놀리식 아키텍처는 소프트웨어 프로그램 설계를 위한 전통적인 통합 모델입니다. 이러한 맥락에서 모놀리식은 모두 한 조각으로 구성된 것을 의미합니다. 모놀리식 소프트웨어는 독립형으로 설계되었습니다. 프로그램의 구성 요소는 모듈식 소프트웨어 프로그램의 경우처럼 느슨하게 결합되지 않고 상호 연결되고 상호 의존적입니다.

위의 두 가지 정의에는 두 가지 공통된 가정이 있습니다.

첫째, 그들은 이 아키텍처가 시스템의 모든 부분이 하나의 배포 단위를 형성한다고 가정한다고 정의합니다. 저도 이에 동의합니다. 이러한 정의에 대한 두 번째 공유 가정은 이러한 아키텍처에서 모듈성이 부족하다고 가정하는 것이며 저는 이에 동의하지 않습니다. '구조적으로 분리된 구성 요소를 포함하기보다는 서로 엮여 있다'와 '프로그램의 구성 요소는 느슨하게 결합되기보다는 상호 연결되고 상호 의존적입니다'라는 문구는 모든 것이 혼합되어 있다고 가정하여 이 아키텍처를 매우 부정적으로 특징짓습니다. 그럴 수도 있지만 "반드시 그럴 필요는 없습니다". 모놀리스의 궁극적인 속성은 아닙니다.

요약하자면, *Monolith*는 **정확히 하나의 배포 단위가 있는 시스템**에 지나지 않습니다. 그 이상도 이하도 아닙니다.

### Modularization

Monolith가 의미하는 바를 정의했습니다. 두 번째 측면인 모듈성으로 가보겠습니다.

[영어 사전](https://dictionary.cambridge.org/dictionary/english/modular)에 따르면 무언가가 모듈화된다는 것은 무엇을 의미합니까?

> Consisting of "separate parts" that, when combined, form a complete whole/made from a set of separate parts that can be "joined together to form a larger object"
>> 결합될 때 완전한 전체를 형성하는 "개별 부분"으로 구성되어 있지만, 이들이 결합될 때 완전한 전체를 형성한다. 분리된 여러 부분들로 구성된 집합으로부터 "더 큰 객체를 형성하기 위해 결합"할 수 있다

모듈화 자체:

> The design or production of something in separate sections
>> 어떤 것을 분리된 섹션으로 디자인하거나 생산하는 것

일반적인 정의이기 때문에 프로그래밍 세계에는 충분하지 않습니다. [모듈식 프로그래밍](https://en.wikipedia.org/wiki/Modular_programming)에 대해 좀 더 구체적인 기술적인 것을 사용해보자.

> Modular programming is a software design technique that emphasizes separating the functionality of a program into "independent, interchangeable modules", such that each "contains everything necessary to execute only one aspect of the desired functionality". A module "interface" expresses the elements that are provided and required by the module. The elements defined in the interface are detectable by other modules. The implementation contains the working code that corresponds to the elements declared in the interface.
>> 모듈형 프로그래밍은 프로그램의 기능을 "독립적이며 상호 교환 가능한 모듈"로 분리하도록 강조하는 소프트웨어 디자인 기법입니다. 이렇게 하면 각 모듈은 "원하는 기능의 일부를 실행하는 데 필요한 모든 것을 포함"하게 됩니다. 모듈 "인터페이스"는 모듈이 제공하고 필요로 하는 요소를 표현합니다. 인터페이스에서 정의된 요소는 다른 모듈에서 감지할 수 있습니다. 구현체는 인터페이스에 선언된 요소에 대응하는 작동 코드를 포함합니다.

여기에서 몇 가지 중요한 문제가 제기되었습니다. 모듈식 아키텍처를 사용하려면 모듈과 다음 모듈이 있어야 합니다.

- a) 독립적이고 상호 교환 가능해야 하며
- b) 원하는 기능을 제공하는 데 필요한 모든 것이 있어야 하며
- c) 정의된 인터페이스가 있어야 합니다.

이러한 가정이 무엇을 의미하는지 봅시다.

#### 모듈은 독립적이고 상호 교환 가능해야 합니다

이 모듈은 그 이름이 암시하듯이 독립적이어야 합니다. 물론 완전히 독립적일 수는 없습니다. 그렇게 되면 다른 모듈과 통합하지 않는다는 의미가 됩니다. 모듈은 항상 어떤 것에 의존할 것이지만, 의존성은 최소화해야 합니다. 즉, [낮은 결합도, 높은 응집도(Loose Coupling, Strong Cohesion)](https://www.kamilgrzybek.com/blog/posts/grasp-explained)원칙을 따라야 합니다.

왼쪽 아래 다이어그램에는 종속성이 많은 모듈이 있으며 이것이 독립적이라고 말할 수는 없습니다. 반면 오른쪽에서는 상황이 반대입니다. 모듈에는 최소한의 종속성이 포함되어 있고 더 느슨하며 마침내 더 독립적입니다.

|![Module independence](/posts/development-architectures/Module_independence-1024x420.jpg)|
|:--:|
|Module independence|
|모듈 독립성|

> - Dependency on Module: 이는 한 모듈이 다른 모듈의 기능이나 서비스에 의존한다는 것을 의미합니다. 예를 들어, 사용자 관리 모듈이 데이터베이스 모듈에 의존할 수 있습니다. 이는 직접적인 의존성으로, 한 모듈이 다른 모듈의 인터페이스나 기능을 직접 호출하게 됩니다. 이 경우 한 모듈의 변화가 다른 모듈에 영향을 줄 수 있으며, 이로 인해 결합도가 높아질 수 있습니다.
> - Dependency on Message Broker: 이는 모듈 간의 의존성이 메시지 브로커라는 중간 매개체를 통해 이루어진다는 것을 의미합니다. 메시지 브로커는 모듈 간에 비동기 메시지를 전달하는 역할을 합니다. 한 모듈은 메시지를 브로커에 게시(publish)하고, 다른 모듈은 이 메시지를 수신(subscribe)합니다. 이 방식을 통해 모듈 간의 직접적인 결합도를 줄일 수 있습니다.
>
> 두 의존성 방식의 주요 차이점은 결합도와 통신 방식에 있습니다. 모듈 의존성은 모듈 간의 직접적인 결합을 초래하며, 일반적으로 동기적인 통신을 사용합니다. 반면에 메시지 브로커를 통한 의존성은 모듈 간의 결합도를 낮추고, 비동기적인 통신을 가능하게 합니다. 이로 인해 시스템의 확장성과 유연성이 향상될 수 있습니다.

그러나 종속성의 수는 모듈이 얼마나 독립적인지를 측정하는 하나의 척도일 뿐입니다. 두 번째 척도는 의존성이 얼마나 강한가입니다. 즉, 여러 메서드를 사용하여 매우 자주 호출합니까, 아니면 가끔 하나 또는 몇 가지 메서드를 사용합니까?

|![Strong/Weak dependency](/posts/development-architectures/Module_independence_strongweak-1024x420.jpg)|
|:--:|
|Strong/Weak dependency|
|강한/약한 종속성|

첫 번째 경우에는 모듈의 경계를 잘못 정의했을 가능성이 있으며 **두 모듈이 밀접하게 관련된 경우 두 모듈을 병합해야 합니다**.

|![Modules merged](/posts/development-architectures/Module_indpendence_merge-1024x420.jpg)|
|:--:|
|Modules merged|
|병합된 모듈|

모듈의 독립성에 영향을 미치는 마지막 속성은 **의존하는 모듈의 변경 빈도**입니다. 짐작할 수 있듯이 변경 빈도가 적을수록 모듈이 더 독립적입니다. 반면에 변경이 자주 발생하면 모듈을 자주 변경해야 하며 독립성을 잃게 됩니다.

|![Module stability](/posts/development-architectures/Module_independence_stability-1024x474.jpg)|
|:--:|
|Module stability|
|모듈 안정성|

요약하면 모듈의 독립성은 세 가지 주요 요소에 의해 결정됩니다.

- 종속성 수
- 종속성의 강도
- 모듈이 의존하는 모듈의 안정성

#### 모듈에는 원하는 기능을 제공하는 데 필요한 모든 것이 있어야 합니다

"모듈(module)"이라는 단어는 많은 의미로 과부하되어 사용될 수 있고, 다양한 상황에서 다른 의미를 가질 수 있습니다. 여기서 일반적인 경우는 논리 계층을 모듈로 호출하는 것입니다. GUI 모듈, 애플리케이션 논리 모듈, 데이터베이스 액세스 모듈. 예, 이 맥락에서 이들은 모듈이기도 하지만 **비즈니스 기능이 아닌 기술을 제공합니다**.

기술적인 측면에서 모듈에 대해 생각할 때, 오직 기술적인 변경만이 정확히 한 모듈을 변경시키게 된다는 내용을 의미합니다.

|![Technical modules and technical change](/posts/development-contents/modular-monolith/TechnicalModules_technicalChange-1024x474.jpg)|
|:--:|
|Technical modules and technical change|
|기술 모듈 및 기술 변경|

비즈니스 기능을 추가하거나 변경하면 일반적으로 모든 계층을 거쳐 **각 기술 모듈이 변경됩니다**.

|![Technical modules - new/change business feature](/posts/development-contents/modular-monolith/TechnicalModule_FeatureChange-1024x421.jpg)|
|:--:|
|Technical modules - new/change business feature|
|기술 모듈 - 신규/변경 비즈니스 기능|

스스로에게 물어봐야 할 질문 다음과 같습니다. "시스템의 기술적 부분 변경이나 비즈니스 기능 변경 중 어떤 변경을 더 자주 수행합니까?" 제 생각에는 - 확실히 더 자주 후자입니다. 데이터베이스 액세스 계층, 로깅 라이브러리 또는 GUI 프레임워크를 거의 교환하지 않습니다. 이러한 이유로 *Modular Monolith*의 모듈은 **원하는 기능 집합을 완벽하게 제공할 수 있는 비즈니스** 모듈입니다. 이러한 종류의 디자인을 "수직 슬라이스"라고 하며 모듈에서 이러한 슬라이스를 그룹화합니다.

|![Business modules and vertical slices](/posts/development-contents/modular-monolith/BusinessModules_VerticalSlices-1-1024x403.jpg)|
|:--:|
|Business modules and vertical slices|
|비즈니스 모듈 및 수직 슬라이스|

이러한 방식으로 빈번한 변경은 **하나의 모듈에만 영향**을 미치며 더 독립적이고 자율적이며 자체적으로 기능을 제공할 수 있습니다.

#### 모듈에는 정의된 인터페이스가 있어야 합니다

모듈성의 마지막 속성은 ***잘 정의된 인터페이스***입니다. 모듈에 계약이 없으면 모듈식 아키텍처에 대해 이야기할 수 없습니다.

|![Modules without contract (interface)](/posts/development-contents/modular-monolith/Modules_without_contract-1024x548.jpg)|
|:--:|
|Modules without contract (interface)|
|계약이 없는 모듈(인터페이스)|

계약은 우리가 외부에서 사용할 수 있도록 하는 것이므로 매우 중요합니다. 모듈의 *"진입점"*입니다. 좋은 계약은 모호하지 않아야 하며 주어진 계약의 클라이언트가 필요로 하는 것만 포함해야 합니다. 클라이언트를 손상시키지 않도록 안정적으로 유지하고 그 뒤에 다른 모든 것을 숨겨야 합니다([캡슐화](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming))).

|![Modules with contract](/posts/development-contents/modular-monolith/Modules_with_contract-1024x548.jpg)|
|:--:|
|Modules with contract|
|계약이 있는 모듈|

위의 다이어그램에서 볼 수 있듯이 모듈의 계약은 다양한 형태를 취할 수 있습니다. 때로는 동기식 호출(예: 공용 메서드 또는 REST 서비스)에 대한 일종의 파사드이고 때로는 비동기식 통신을 위해 게시된 이벤트일 수 있습니다. 어쨌든 우리가 외부에서 공유하는 모든 것은 **모듈의 공개 API가 됩니다**. 따라서 **캡슐화는 모듈화의 불가분의 요소입니다**.

## Summary

1. Monolith는 정확히 하나의 배포 단위가 있는 시스템입니다.
1. 모놀리식 아키텍처는 시스템이 잘못 설계되었거나 모듈식이거나 나쁘다는 것을 의미하지 않습니다. 품질에 대해서는 아무 말도 하지 않습니다.
1. 모듈러 모놀리스 아키텍처는 모듈 방식으로 설계된 모놀리스 시스템의 명시적 이름입니다.
1. 높은 수준의 모듈화를 달성하려면 각 모듈이 독립적이어야 하고 원하는 기능을 제공하는 데 필요한 모든 것이 있어야 하며(사업 영역별 분리) 캡슐화되고 잘 정의된 인터페이스가 있어야 합니다.

다음 게시물에서는 Modular Monolith 아키텍처를 마이크로 서비스와 비교하여 장단점에 대해 논의할 것입니다.

## Additional resources

1. [Modular Monoliths Video - Simon Brown](https://www.youtube.com/watch?v=5OjqD-ow8GE).
1. [Majestic Modular Monoliths - Axel Fontaine](https://www.youtube.com/watch?v=BOvxJaklcr0).
1. [Modular programming - Wikipedia](https://en.wikipedia.org/wiki/Modular_programming).
1. [Monolithic application - Wikipedia](https://en.wikipedia.org/wiki/Monolithic_application).
1. [Modular Monolith with DDD - GitHub repository](https://github.com/kgrzybek/modular-monolith-with-ddd).
1. [Vertical Slice Architecture - Jimmy Bogard](https://jimmybogard.com/vertical-slice-architecture/).

Image credits: [Magnasoma](https://magnasoma.com)
