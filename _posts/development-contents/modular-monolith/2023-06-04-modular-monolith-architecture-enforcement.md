---
title: MODULAR MONOLITH - ARCHITECTURE ENFORCEMENT (모듈러 모놀로스 - 아키텍처 시행)
author: June
date: 2023-06-04 13:50:00 +0900
categories: [개발 게시물, MODULAR MONOLITH]
tags: [아키텍처, 모듈러 모놀리스 아키텍처]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Kamil Grzybek](https://www.kamilgrzybek.com/about)의 포스트 [MODULAR MONOLITH: ARCHITECTURE ENFORCEMENT](https://www.kamilgrzybek.com/blog/posts/modular-monolith-architecture-enforcement)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Introduction

이전 포스트에서 우리는 모듈러 모놀리스의 아키텍처와 그 선택에 영향을 미칠 수 있는 아키텍처의 드라이버에 대해 논의했습니다. 이 포스트에서는 "선택한 아키텍처를 강제하는" 방법에 초점을 맞추고 싶습니다.

아래에 설명된 방법들은 단지 모듈러 모놀리스 아키텍처에 대한 것만이 아니라, 보편적으로 적용될 수 있습니다. 하지만, 모노리식에서는 모노리식의 특성과 코드베이스의 크기 그리고 변경 용이성과 같은 점들 때문에, 아키텍처를 강제하는 것이 특히 중요합니다.

## Model-code gap

현재의 아키텍처 드라이버에 기반하여, *모듈러 모놀리스* 아키텍처를 결정했다고 가정해봅시다. 또한 모듈 경계와 솔루션 아키텍처를 미리 정의했다고 가정해봅시다. 기술, 접근법, 모듈 간의 통신 방법, 지속성 방법이 선택되었습니다.

모든 것을 솔루션 아키텍처 문서/설명([SAD](https://en.wikipedia.org/wiki/Software_architecture_description))의 형태로 문서화하었거나, 몇 개의 다이어그램(UML, C4 모델 또는 간단한 화살표와 박스를 사용)을 만들었습니다. 충분한 사전 설계를 했으므로, 구현을 시작할 수 있습니다.

처음에는 매우 단순합니다. 기능이 많지 않고, 코드도 적으며, 쉽게 유지 관리되고 모델링된 아키텍처와 일관성이 있습니다. 시간이 충분하고, 무언가 잘못되더라도 리팩토링하기 쉽습니다. 지금까지는 모든 것이 잘 진행되었습니다.

그러나, 어느 순간부터는 더 이상 쉽지 않습니다. 기능과 코드가 늘어나고, 요구사항 변경이 시작되며, 마감일이 쫓아옵니다. 우리는 지름길을 만들기 시작하고, 우리의 구현은 설계와 크게 다르게 됩니다. *모듈러 모놀리스* 아키텍처의 경우, 가장 빈번하게 이런 방식으로 모듈성, 독립성을 잃게 되고, 모든 것이 모든 것과 통신하기 시작합니다. 또 다른 [큰 진흙 더미](https://en.wikipedia.org/wiki/Big_ball_of_mud)가 만들어집니다:

![BBoM](/posts/development-contents/modular-monolith/BBoM.jpg)

> It was supposed to be like never before, it ended as always
>> 예전과 같지 않았을 텐데 언제나처럼 끝났습니다

George Fairbanks는 그의 책 [Just Enough Software Architecture: A Risk-Driven Approach](https://www.amazon.com/Just-Enough-Software-Architecture-Risk-Driven/dp/0984618104)에서 위에서 설명한 현상을 다음과 같이 정의합니다:

> Your architecture models and your source code will not show the same things. The difference between them is the model-code gap.
>> 당신의 아키텍처 모델과 소스 코드는 같은 것을 보여주지 않을 것입니다. 그들 사이의 차이가 바로 모델-코드 간극(model-code gap)입니다.

그리고 나중에:

> Whether you start with source code and build a model, or do the reverse, you must manage two representations of your solution. Initially, the code and models might correspond perfectly, but over time they tend to diverge. Code evolves as features are added and bugs are fixed. Models evolve in response to challenges or planning needs. Divergence happens when the evolution of one or the other yields inconsistencies.
>> 소스 코드로 시작하여 모델을 만들거나, 그 반대를 하는 경우, 당신은 솔루션의 두 가지 표현을 관리해야 합니다. 초기에는 코드와 모델이 완벽하게 일치할 수 있지만, 시간이 지남에 따라 그들은 다양해집니다. 기능이 추가되고 버그가 수정됨에 따라 코드는 진화합니다. 모델은 도전이나 계획 필요성에 대응하여 진화합니다. 한쪽이나 다른 쪽의 진화가 불일치를 초래할 때 발산이 발생합니다.

우리는 장기적으로 보면 항상 이런 종말에 처하게 될까요? 그런건 아닙니다. 확실히 우리 자신에게 많은 자기극복이 필요하겠지만, 그것만이 전부는 아닙니다. 우리의 아키텍처를 체크하게 하는 적절한 실천법과 접근법을 적용해야 합니다. 그럼 이런 접근법은 무엇인가요?

## Architecture enforcement

우리의 구현이 가정한 설계와 일치하는지 확인하기 위한 도구를 설명할 때, 우리는 2가지 측면을 고려해야 합니다.

첫 번째 측면은 도구가 우리에게 제공하는 "가능성"입니다. 우리가 알고 있듯이 아키텍처는 다른 추상화 수준에서의 규칙들의 집합이며, 때때로 정의하기 어렵습니다. 물론 우리가 그것들을 확인해야 한다는 점은 더욱이요.

두 번째 측면은 우리가 "얼마나 빨리 피드백을 받는지"입니다. 물론, 빠를수록 좋습니다. 왜냐하면 우리가 무언가를 더 빨리 수정할 수 있기 때문입니다. 무언가를 더 빨리 수정하면, 이 오류가 나중에 우리의 아키텍처에 미치는 영향이 줄어듭니다.

다음 가정을 고려할 때, 아키텍처 강제에 대해서는 컴파일러(compiler), 자동 테스트(automated tests), 코드 리뷰(code review)라는 3가지 다른 수준에서 이를 수행할 수 있습니다.

|![Architecture enforcement approaches](/posts/development-contents/modular-monolith/Modular-Monolith_-Architecture-Enforcement-Enforcement-tools-1024x642.jpg)|
|:--:|
|Architecture enforcement approaches|
|아키텍처 시행 접근 방식|

## Compile-time

**컴파일러는 당신의 최고의 친구입니다**. 컴파일러를 통해 당신이 오랜 시간을 들여서 확인해야 할 많은 사항들을 빠르게 확인할 수 있습니다. 더불어, 컴파일러는 틀릴 수 없지만, 사람은 틀릴 수 있습니다. 그렇다면, 왜 우리는 우리가 선택한 아키텍처의 준수를 위해 컴파일러를 적극적으로 사용하지 않는 것일까요? 왜 우리는 컴파일러의 가능성을 최대한 활용하려고 하지 않는 것일까요?

첫 번째 주요한 잘못 된 점은 모든 것이 "공개적"이라는 원칙입니다. 모듈성의 정의에 따르면, 모듈은 잘 정의된 인터페이스를 통해 통신해야 하며, 이는 그들이 캡슐화되어야 함을 의미합니다. 만약 모든 것이 공개적이라면, 캡슐화는 없습니다.

불행히도, 프로그래밍 커뮤니티는 다음을 통해 이 현상을 선호합니다:

- 튜토리얼
- 샘플 프로젝트
- IDE (기본적으로 공개 클래스를 생성)

우리는 확실히 "기본적으로 비공개"라는 접근 방식을 바꾸어야 합니다. 만약 무언가가 비공개일 수 없다면, 그것은 모듈의 범위 내에서 사용 가능하게 하되, 여전히 다른 사람들에게는 접근할 수 없게 해야 합니다.

어떻게 해야 할까요? 불행하게도, .NET에서는 우리의 선택지가 제한적입니다. 우리가 할 수 있는 유일한 것은 모듈을 별도의 어셈블리로 분리하고 "internal" 접근 제어자를 사용하는 것입니다. 하나의 프로젝트(어셈블리)에 모든 코드를 넣는 것을 지지하는 사람들과 많은 프로젝트로 분할하는 것을 지지하는 사람들 사이에는 거의 전쟁이 벌어지고 있습니다.

전자들은 어셈블리가 구현 단위라고 주장합니다. 그렇지만, 우리가 모듈을 캡슐화할 다른 방법이 없기 때문에, 프로젝트로의 분할은 합리적인 해결책으로 보입니다. 더불어, 참조를 확인함으로써, 잘못된 의존성(예: 도메인에서 인프라로의 의존성)을 추가하는 것이 어렵거나 심지어 불가능해집니다.

캡슐화의 부재는 제가 보는 가장 흔한 잘 못된 점 중 하나이지만 유일한 것은 아닙니다. 다른 것들로는 불변성을 사용하지 않는 것(불필요한 setter)이나 강한 타입을 사용하지 않는 것(원시적인 강박증(primitive obsession)) 등이 있습니다.

일반적으로 말해서, 우리는 우리의 언어를 컴파일러가 가능한 많은 실수를 잡아낼 수 있도록 사용해야 합니다. 이것이 시스템의 아키텍처를 강제하는 가장 효과적인 접근법입니다.

|![Architecture enforcement - compile-time](/posts/development-contents/modular-monolith/Modular-Monolith_-Architecture-Enforcement-Compile-time-1024x555.jpg)|
|:--:|
|Architecture enforcement - compile-time|
|아키텍처 적용 - 컴파일 시간|

## Automated tests

모든 것을 컴파일러를 사용하여 확인할 수는 없습니다. 그러나 이는 우리가 그것을 수동으로 확인해야 한다는 의미는 아닙니다. 반대로, 컴퓨터는 여전히 우리를 위해 그것을 수행할 수 있습니다. 이 경우, 우리는 2가지 메커니즘 - 정적 코드 분석과 자동화된 테스트 - 를 사용할 수 있습니다.

### Static code analysis

더 익숙하고 일반적인 방법인 정적 코드 분석기를 사용하는 것입니다.. 확실히, 대부분의 사람들은 [SonarQube](https://www.sonarqube.org/sonarqube-8-2/)나 [NDepend](https://www.ndepend.com/)같은 도구들에 대해 들어본 적이 있을 것입니다. 이들은 우리의 코드에 대해 자동으로 정적 분석을 수행하고, 그 결과를 바탕으로 매우 유용할 수 있는 메트릭 정보를 제공하는 도구입니다. 물론, 정적 코드 분석기를 CI 프로세스에 연결하고 정기적으로 피드백을 받을 수 있습니다.

|![Architecture Enforcement - static analysis](/posts/development-contents/modular-monolith/Modular-Monolith_-Architecture-Enforcement-Copy-of-Compile-time-1024x292.jpg)|
|:--:|
|Architecture Enforcement - static analysis|
|아키텍처 적용 - 정적 분석|

### Architecture tests

아키텍처 테스트는 덜 알려져 있지만 인기를 얻고 있는 또 다른 방법입니다. 이것들은 유닛 테스트이지만, 비즈니스 기능을 테스트하는 대신에 아키텍처의 맥락에서 우리의 코드베이스를 테스트합니다. 대부분, 이러한 테스트들은 이 유형의 테스트에 전용된 라이브러리를 기반으로 작성됩니다. 아키텍처 테스트는 다음과 같습니다:

```csharp
// Architecture test - value object should be immutable
// 아키텍처 테스트 - 값 개체는 변경할 수 없어야 합니다.
[Test]
public void ValueObject_Should_Be_Immutable()
{
    var types = Types.InAssembly(DomainAssembly)
        .That()
        .Inherit(typeof(ValueObject))
        .GetTypes();

    AssertAreImmutable(types);
}
```

```csharp
// Architecture test - domain layer does not have dependency to infrastructure
// 아키텍처 테스트 - 도메인 계층은 인프라에 대한 종속성이 없습니다.
[Test]
public void DomainLayer_DoesNotHaveDependency_ToInfrastructureLayer()
{
    var result = Types.InAssembly(DomainAssembly)
        .Should()
        .NotHaveDependencyOn(ApplicationAssembly.GetName().Name)
        .GetResult();

    AssertArchTestResult(result);
}
```

이러한 테스트들을 이용하면 많은 것을 확인할 수 있습니다. 이것을 위한 라이브러리들(예: [NetArchTests](https://github.com/BenMorris/NetArchTest) 또는 [ArchUnit](https://www.archunit.org/))은 많은 기능을 제공하며, 다양한 테스트들을 작성하는 것은 어렵지 않습니다. 이러한 테스트를 사용하는 완전한 예시는 [여기](https://github.com/kgrzybek/modular-monolith-with-ddd/tree/master/src/Modules/Meetings/Tests/ArchTests)에서 확인하실 수 있습니다.

|![Architecture Enforcement – architecture tests](/posts/development-contents/modular-monolith/Modular-Monolith_-Architecture-Enforcement-Architecture-tests-1024x620.jpg)|
|:--:|
|Architecture Enforcement – architecture tests|
|아키텍처 적용 - 아키텍처 테스트|

## Code review

만약 우리가 컴퓨터(컴파일러, 자동화된 테스트)를 사용하여 선택한 아키텍처와 우리의 솔루션의 일치성을 확인할 수 없다면, 마지막 도구로 코드 리뷰가 있습니다. 코드 리뷰 덕분에, 우리는 컴퓨터가 우리를 위해 할 수 없는 모든 것을 확인할 수 있습니다. 하지만, 이것은 몇 가지 단점도 가지고 있습니다.

첫 번째 단점은 사람들이 틀릴 수 있다는 것입니다. 그래서 아키텍처 결정 사항이 지켜지지 않은 부분을 놓칠 확률이 상대적으로 높습니다.

두 번째 단점은, 코드 리뷰에 투자해야 할 시간이 많다는 것입니다. 물론, 이것은 시간 낭비가 아니며 우리는 이것을 포기할 수 없습니다. 그렇기 떄문에 코드 리뷰에 투자되는 시간은 항상 프로젝트의 추정치에 포함되어야 합니다.

결론은 명확합니다 - 아키텍처를 강제하기 위해 가능한 한 많이 컴퓨터를 사용해야 하며 코드 리뷰를 마지막 방어선으로 취급해야 합니다. 질문은 이 방어선을 어떻게 강화할 것인가, 즉 코드 리뷰 중에 무언가를 놓칠 확률과 시간을 어떻게 줄일 것인가에 대한 것입니다. 우리는 *Architecture Decisions Records (ADR)*를 사용할 수 있습니다.

### Architecture Decisions Records (ADR)

*아키텍처 결정 기록(Architecture Decisions Records)*이란 무엇일까요? 이 주제와 관련된 가장 인기 있는 [GitHub 저장소](https://github.com/joelparkerhenderson/architecture_decision_record)에서 정의는 다음과 같습니다:

> An architectural decision record (ADR) is a document that captures an important architectural decision made along with its context and consequences.
>> 아키텍처 결정 기록 (ADR)은 그 컨텍스트 및 결과와 함께 이루어진 중요한 아키텍처 결정을 기록하는 문서입니다.

이런 종류의 문서는 보통 버전 관리 시스템에 저장되며, ThoughtWorks 회사의 인기 있는 [technology radar](https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records)에서도 이 방법을 권장합니다.

제 조언은 가능한 한 간단하고 빠르게 결정을 기술하는 것 부터 시작하라는 것입니다. 불필요한 양식 없이, 가장 중요한 요소인 컨텍스트, 결정 사항 그리고 결과가 들어 있는 간단한 템플릿(예: [Michael Nygard가 제안한 것](https://github.com/joelparkerhenderson/architecture-decision-record/blob/main/templates/decision-record-template-by-michael-nygard/index.md))을 선택하세요. 그런데, 아키텍처 결정 기록이 어떻게 코드 리뷰와 관련이 있을까요?

첫째, 모든 결정이 공개되며, 모두가 그들에게 접근하고 설명할 수 있습니다. "*나는 몰랐었다*"라고 말하는 그런 것은 없습니다. 이러한 결정들은 정의상 중요하므로, **모두가 그들을 알고 따라야 합니다**.

둘째는, 코드 리뷰 과정을 가속화시킨다는 것입니다. 왜 무언가가 잘못되었는지 쓰는 대신에, 우리가 왜 그렇게 하는지, 결정이 무엇이었는지, 언제 그리고 어떤 맥락에서 그렇게 했는지 설명하는 대신에 적절한 *ADR*에 대한 링크를 붙여넣을 수 있습니다.

## Summary

모든 시스템은 어떠한 아키텍처를 가지고 있습니다. 문제는: **당신이 시스템의 아키텍처를 형성할 것인지, 아니면 그것이 스스로 형성될 것인지입니다**? 확실히 첫 번째 옵션이 더 좋습니다. 왜냐하면 두 번째 옵션은 우리를 큰 실패로 몰아넣을 수 있기 때문입니다.

아키텍처를 강제하는 것은 (아키텍트만이 아닌)모든 팀원의 책임입니다. 그래서 우리가 그것을 어떻게 하는지가 매우 중요합니다. 이것은 헌신이 필요한 과정입니다. 제가 언급한 기법들은 아키텍처 강제의 과정을 상당히 용이하게 하고 향상시키는 동시에, 우리 시스템의 품질을 적절한 수준에서 유지할 수 있도록 도와줍니다.

## Additional Resources

1. [Unit Test Your Architecture with ArchUnit - Jonas Havers, article](https://blogs.oracle.com/javamagazine/unit-test-your-architecture-with-archunit)
1. [Architecture Decision Records in Action presentation - Michael Keeling, Joe Runde, presentation](https://resources.sei.cmu.edu/asset_files/Presentation/2017_017_001_497746.pdf)
1. [Design It! - Michael Keeling, book](https://www.oreilly.com/library/view/design-it/9781680502923/)
1. [Modular Monolith with DDD - Kamil Grzybek, GitHub repository](https://github.com/kgrzybek/modular-monolith-with-ddd)
1. [“Modular Monoliths” - Simon Brown, video](https://www.youtube.com/watch?v=5OjqD-ow8GE)

Image credits: [nanibystudio](https://magnasoma.com/)
