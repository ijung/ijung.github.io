---
title: "JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?"
date: 2026-11-02 13:25:54 +0900
categories: [AI, 기술]
tags: [jev, typesafe, system-1, system-2, ai-workflow]
toc: true
comments: false
permalink: /posts/jev-system-1-vs-system-2/
---
> 모든 판단에 긴 추론이 필요한 것은 아니다. 그렇다고 빠른 판단만으로 모든 문제를 해결할 수도 없다.

고객센터에 이런 메시지가 도착했다고 해보자.

“같은 주문이 두 번 결제됐어요. 확인하고 환불해주세요.”

이 문의를 결제 담당 팀으로 보내는 일과, 결제 기록·환불 규정·예외 조항을 검토해 실제 환불 여부를 결정하는 일은 다르다. 전자는 비교적 좁은 분류 문제다. 후자는 사실 확인과 정책 검토, 때로는 사람의 승인이 필요한 업무다.

둘 다 AI에게 맡길 수 있는 부분이 있지만, 꼭 같은 방식으로 처리해야 할까?

TypeSafe가 개발한 Jev는 이 질문을 던지기에 흥미로운 모델이다. 회사는 Jev를 **빠르고 구조화된 판단을 소프트웨어에 전달하는 ‘System One 모델’**로 소개한다.<a href="#source-1">[1]</a> 자연어를 이해하지만, 답변 문장을 쓰는 대신 정해진 형태의 판단과 확률을 반환한다.<a href="#source-1">[1]</a>

이번 글은 JEV를 살펴보는 연재의 첫 편이다. 세부 API나 성능 수치에 앞서, 이름의 출발점인 **System 1과 System 2**부터 짚어보자.

## 1. System 1과 System 2: 속도보다 중요한 차이

다니엘 카너먼의 《Thinking, Fast and Slow》를 통해 널리 알려진 구분에서 System 1은 빠르고 직관적인 사고, System 2는 느리고 숙고하는 사고를 가리킨다.<a href="#source-1">[1]</a> TypeSafe도 이 구분에서 System One이라는 이름을 가져왔다고 설명한다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

일상적인 장면으로 옮겨보면 이렇다.

- **System 1에 가까운 장면:** 익숙한 표정을 보고 상대의 기분을 짐작한다. 짧은 문의를 읽고 어떤 부서의 일인지 알아챈다.
- **System 2에 가까운 장면:** 서로 충돌하는 계약 조건을 비교한다. 여러 가정을 세워 문제를 검토하고 처음 판단을 수정한다.

다만 ‘빠르면 System 1, 느리면 System 2’라는 설명만으로는 부족하다. Evans와 Stanovich의 이중과정 이론 논문은 Type 1의 자율적인 처리와 Type 2의 작업기억 의존성·가설적 사고를 중요한 구분으로 제시한다.<a href="#source-5">[5]</a>

따라서 이 글에서는 두 용어를 **빠르게 떠오르는 판단과 의식적인 검토의 차이를 이해하는 출발점**으로 사용한다. 사람의 머릿속에 정확히 두 개의 독립된 엔진이 있다고 가정하거나, 하나는 늘 틀리고 다른 하나는 늘 옳다고 나누려는 것이 아니다.<a href="#source-5">[5]</a>

## 2. AI에 적용할 때는 ‘비유’라는 선을 지켜야 한다

여기서 곧바로 이렇게 정리하고 싶어질 수 있다.

> Jev는 System 1이고, LLM은 System 2다.

이 글에서는 그런 일대일 대응을 사용하지 않는다. 인간의 인지과정을 설명하는 이론과 AI 제품의 설계 목표는 서로 다른 층위의 이야기이기 때문이다. TypeSafe가 System One이라는 이름을 쓴다는 사실이 인간의 직관을 그대로 구현했다는 증거는 아니다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a>

대신 **소프트웨어 안에서 역할을 나누는 비유**로 읽어보자.

- 빠른 판단 경로: 주어진 맥락에서 범위가 좁은 질문에 답한다.
- 숙고·검토 경로: 여러 조건을 종합하고 예외를 검토하며 필요하면 추가 정보를 구한다.

이것은 모델의 이름보다 시스템을 어떻게 설계할지에 초점을 맞춘 구분이다. 한 모델이 모든 일을 맡아야 한다는 전제도 없다.

중요한 질문은 “누가 더 오래 생각하는가?”가 아니라 **“이 단계에서는 무엇을 판단해야 하고, 그 판단만으로 다음 행동을 해도 되는가?”**다.


<figure>
  <img src="/assets/img/posts/jev-system-1-vs-system-2/judgment-and-review.png" alt="빠르고 좁은 분류 판단과 맥락·예외를 확인하는 숙고 검토를 세로로 배치하고, 빠른 판단은 실행 권한이 아니라고 설명하는 개념도" width="720" height="1120" loading="lazy" style="max-width:100%;height:auto;">
  <figcaption>그림 1. 빠른 판단과 숙고·검토의 역할 분담을 설명하기 위해 직접 제작한 개념도다. 인간의 인지 구조나 Jev의 내부 구조를 묘사한 것이 아니며 실측 성능 비교도 아니다. <a href="#source-1">[1]</a><a href="#source-5">[5]</a> <a href="/assets/img/posts/jev-system-1-vs-system-2/judgment-and-review.png">원본 이미지 보기</a></figcaption>
</figure>

## 3. Jev는 답변보다 ‘판단 결과’를 돌려준다

Jev의 공식 문서가 강조하는 차이는 출력에 있다.<a href="#source-1">[1]</a> 자유로운 답변·코드·추론 설명을 생성하는 대신, 개발자가 정의한 질문에 제한된 형태의 값을 반환한다.<a href="#source-1">[1]</a>

현재 문서에 소개된 기본 질문 유형은 다음과 같다.<a href="#source-3">[3]</a>

- **Choice:** 정해진 선택지 중 무엇에 해당하는가?<a href="#source-3">[3]</a> 예를 들면 문의를 결제·기술·계정 담당 중 어디로 보낼지 판단한다.
- **Score:** 정의된 단계 중 어느 수준인가?<a href="#source-3">[3]</a> 예를 들면 고객의 불만 정도를 미리 정한 척도에 놓는다.
- **Noul:** 어떤 명제가 참일 확률은 얼마인가?<a href="#source-3">[3]</a> 예를 들면 메시지가 환불을 요청하는지 판단한다.

처음의 고객 문의로 돌아가 보자. 이 단계에서 필요한 것은 긴 설명문이 아닐 수 있다.

“고객이 환불을 요청하고 있으며, 결제 관련 문의에 해당하므로…”라는 문단 대신, **문의 유형과 환불 요청 여부를 코드가 바로 검사할 수 있는 값으로 받는 방식**이다.

물론 기존 LLM도 구조화된 출력을 지원한다.<a href="#source-6">[6]</a> OpenAI의 Structured Outputs가 한 사례다.<a href="#source-6">[6]</a> 그러므로 Jev를 ‘처음으로 JSON을 반환하는 AI’처럼 소개하는 것은 적절하지 않다.

차별점은 단순한 출력 포맷보다 **자유 텍스트 생성을 하지 않고, 제한된 판단과 확률을 중심으로 설계했다는 제품 방향**에 있다.<a href="#source-1">[1]</a> TypeSafe는 이를 위해 RLCD(Reinforcement Learning for Calibrated Decisions)라는 학습 방식과 병렬 샘플링을 사용한다고 설명한다.<a href="#source-2">[2]</a> 여기까지는 회사의 기술 설명이며, 이 글에서 내부 구현이나 성능을 독립 검증한 것은 아니다.

## 4. ‘vs’보다 중요한 것은 역할 분담이다

이 관점에서 생각하면 System 1 vs System 2는 승자를 고르는 대결보다 **어떤 일을 어느 경로에 맡길지 정하는 설계 문제**에 가깝다.

다음은 고객 문의 처리를 위한 개념 설계다. 실제 Jev 실행 결과가 아니라, 공식 문서의 워크플로 설명을 바탕으로 구성한 예시다.<a href="#source-1">[1]</a>


<figure>
  <img src="/assets/img/posts/jev-system-1-vs-system-2/inquiry-policy-workflow.png" alt="고객 문의와 업무 맥락에서 좁은 판단을 거친 뒤 코드로 정책·권한을 검사하고, 조건 충족이면 허용 처리, 모호하거나 고위험이면 추가 검토와 사람 승인으로 분기하는 흐름" width="720" height="1460" loading="lazy" style="max-width:100%;height:auto;">
  <figcaption>그림 2. 공식 워크플로 설명을 참고해 직접 제작한 문의 처리 개념도다. 실제 API 실행 화면·실측 결과·Jev 내부 아키텍처가 아니다. <a href="#source-1">[1]</a><a href="#source-3">[3]</a> <a href="/assets/img/posts/jev-system-1-vs-system-2/inquiry-policy-workflow.png">원본 이미지 보기</a></figcaption>
</figure>


여기서 빠른 판단이 맡을 수 있는 질문은 “환불을 요청하는가?”, “어느 담당 팀의 일인가?”처럼 범위가 좁다. 반면 “이 상황에서 최선의 대응을 모두 분석하라”는 요청은 여러 질문으로 나누거나 별도의 검토 경로에서 다루는 편이 맞다. Jev 문서도 이런 식으로 작은 질문을 만들고 결과를 코드에서 조합하도록 권한다.<a href="#source-3">[3]</a>

**환불을 원한다는 판단과 환불해도 된다는 권한은 다르다.**

이는 이 글에서 제안하는 설계 원칙이다. 결제 기록, 중복 처리 여부, 승인 한도와 같은 조건은 별도로 확인해야 한다. 모델이 어떤 답을 자신 있게 내놓았다는 이유만으로 실행 권한까지 주어서는 안 된다.

복잡한 사례를 reasoning 모델로 넘기는 것도 최종 정답을 보장하는 장치는 아니다. 추가 검토 경로를 마련하는 것이며, 위험에 따라 사람의 판단이나 별도 검증을 남겨두어야 한다.

## 5. 빠른 판단을 믿기 전에 구분할 것들

### 형식이 맞는 답과 올바른 답

결제·기술·계정 중 하나만 반환하도록 제한했다고 해보자. 모델이 허용되지 않은 부서명을 만들어내지 않는 것과, 실제로 올바른 부서를 고르는 것은 다른 문제다.

TypeSafe 소개글은 스키마 준수를 강하게 강조하며 ‘환각 없음’이라는 표현을 쓴다.<a href="#source-2">[2]</a> 하지만 같은 글은 제시한 0% 수치가 경험적으로 측정한 오류율이 아니라 스키마 보장에서 나온 것이라고 설명한다.<a href="#source-2">[2]</a> 따라서 이를 **‘업무 판단 오류가 없다’는 뜻으로 읽어서는 안 된다.**

### 확률과 confidence

현재 문서에서 Choice와 Score의 confidence는 출력 확률분포를 요약하는 값이다.<a href="#source-7">[7]</a> Noul은 참일 확률을 반환하며 별도 confidence 필드는 없다.<a href="#source-3">[3]</a><a href="#source-7">[7]</a>

따라서 confidence를 그대로 ‘이 답이 맞을 확률’이라고 읽으면 안 된다. TypeSafe 역시 calibration, 즉 확률 보정은 여러 예측의 집단 수준에서 측정하는 성질이며 개별 답의 정답을 보장하지 않는다고 명시한다.<a href="#source-1">[1]</a>

실무에서는 자신의 데이터로 오분류와 불확실성 처리를 평가하고, 어떤 경우 자동 처리하지 않을지 정해야 한다. 특히 공식 모델 문서는 영어에서 정확도가 가장 좋으며 다른 언어는 동일한 수준이 아니라고 설명한다.<a href="#source-4">[4]</a> 한국어 업무라면 한국어 사례로 검증하는 과정이 필요하다.

### 회사의 평가와 우리 서비스의 성능

TypeSafe는 자체 워크플로 평가에서 큰 속도·비용 이점을 보고한다.<a href="#source-2">[2]</a> 동시에 해당 워크플로가 자사 팀이 만든 것이고, 공개한 이득이 실제 환경에서 얻을 수 있는 이득의 높은 쪽일 수 있다는 단서를 붙인다.<a href="#source-2">[2]</a>

따라서 도입 판단의 질문은 “몇 배 빠르다고 하는가?”에서 끝나면 안 된다. 우리 업무에서도 정확도가 충분한지, 모호한 사례를 잘 넘기는지, 검토와 재처리까지 포함한 총비용이 줄어드는지를 확인해야 한다. 이 글에서는 성능 실험이나 유료 API 호출을 수행하지 않았다.

## 6. 더 오래 생각하는 AI만 필요한 것은 아니다

Jev를 System 1과 System 2의 관점에서 읽으며 얻는 가장 유용한 질문은 이것이다.

> **모든 단계에 같은 깊이의 추론이 필요한가?**

어떤 단계에서는 좁은 질문에 빠르게 답하는 능력이 중요하다. 다른 단계에서는 처음의 판단을 의심하고, 추가 맥락을 모으고, 예외를 검토하는 과정이 중요하다.

이 글의 결론은 둘 중 하나를 버리자는 것이 아니다. **빠른 판단으로 충분한 일과 더 깊은 검토가 필요한 일을 구분하자**는 것이다.

그렇게 보면 Jev의 흥미로운 지점은 ‘생각하지 않는 AI’가 아니라, **판단 결과를 제한된 형태로 반환하고 그 결과를 어떻게 사용할지는 소프트웨어 설계의 문제로 남겨둔다는 점**이다.

System 1 vs System 2라는 제목으로 시작했지만, 실제로 설계해야 할 것은 둘의 경쟁보다 역할 분담일지 모른다.

다음 편에서는 이 역할 분담의 재료인 **Choice·Score·Noul**을 살펴보려 한다. 자연어로 된 업무를 어떤 작은 질문들로 나누면 좋은지, 또 나누면 안 되는 의존성은 무엇인지가 주제다.

---

자료 확인일: 2026년 11월 2일(KST). 공개 문헌을 바탕으로 한 해설이며, 실제 Jev 호출이나 독립 성능 재현은 수행하지 않았다. TypeSafe 자료는 제품 제공자의 설명이며 독립 성능 검증과 구분한다.

## Sources

[1] <span id="source-1"></span> <a href="https://docs.typesafe.ai/concepts/system-one">TypeSafe · System One</a>

[2] <span id="source-2"></span> <a href="https://typesafe.ai/blog/introducing-system-one-models-and-jev">TypeSafe · Introducing System One Models &amp; Jev</a>

[3] <span id="source-3"></span> <a href="https://docs.typesafe.ai/primitives">TypeSafe · Primitives (Questions)</a>

[4] <span id="source-4"></span> <a href="https://docs.typesafe.ai/models">TypeSafe · Models</a>

[5] <span id="source-5"></span> <a href="https://scottbarrykaufman.com/wp-content/uploads/2014/04/dual-process-theory-Evans_Stanovich_PoPS13.pdf">Evans &amp; Stanovich · Dual-Process Theories of Higher Cognition: Advancing the Debate</a>

[6] <span id="source-6"></span> <a href="https://openai.com/index/introducing-structured-outputs-in-the-api">OpenAI · Introducing Structured Outputs in the API</a>

[7] <span id="source-7"></span> <a href="https://docs.typesafe.ai/confidence">TypeSafe · Confidence</a>
