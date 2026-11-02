---
title: "JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?"
date: 2026-11-02 16:16:08 +0900
categories: [AI, 기술]
tags: [jev, typesafe, ai-workflow]
toc: true
comments: false
permalink: /posts/jev-structure-vs-llm/
---

<nav id="jev-series-nav" aria-label="JEV 탐구 연재 nav">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><strong aria-current="page">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</strong></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><a href="/posts/jev-strengths-limits-trust/">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</a></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><a href="/posts/jev-adoption-system-design/">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</a></li>
  </ol>
  <p><a href="/posts/jev-system-1-vs-system-2/">← 이전 편</a> · <a href="/posts/jev-designing-small-questions/">다음 편 →</a></p>
</nav>

> Jev와 LLM의 차이를 ‘문장 대 JSON’으로만 설명하면, 정작 중요한 차이를 놓치게 됩니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>

“같은 주문이 두 번 결제됐어요. 확인하고 환불해주세요.”

1편에서 만난 고객 문의입니다. 담당 팀을 정할 때 필요한 것은 분류 결과이지만, 고객에게 상황을 설명할 때는 문장이 필요합니다. 실제 환불에는 결제 기록과 정책, 실행 권한도 필요합니다. 같은 메시지를 읽더라도 소프트웨어가 받아야 할 결과는 업무 단계마다 다릅니다.<a href="#source-1">[1]</a>

TypeSafe는 Jev를 자연어를 이해하되 생성 문장 대신 **타입이 정해진 판단과 확률을 반환하는 모델**로 설명합니다.<a href="#source-1">[1]</a>
그렇다면 기존 LLM에 JSON으로 답하라고 시키는 것과 무엇이 다를까요? 답을 얻으려면 입력·출력 계약, 회사가 밝힌 처리 방식, 학습 목표를 나누어 살펴봐야 합니다. 이 글의 ‘구조’는 우선 **외부에서 사용하는 구조**를 뜻하며, 비공개 신경망의 설계도를 뜻하지 않습니다.

## 1. 입력은 여전히 자연어입니다

Jev가 문장을 생성하지 않는다고 해서 자연어를 읽지 못하는 것은 아닙니다.<a href="#source-1">[1]</a>
현재 공식 문서는 텍스트 입력을 지원하며, 문자열뿐 아니라 JSON 객체와 텍스트 배열도 평가한다고 설명합니다.<a href="#source-1">[1]</a>
이미지·음성·영상 입력은 현재 지원 범위가 아닙니다.<a href="#source-1">[1]</a>

문서에서 **State**는 모델이 판단할 재료를 뜻합니다.<a href="#source-2">[2]</a>
문의 한 줄도 State가 될 수 있고, 대화·주문 기록·정책을 묶은 객체도 하나의 State가 될 수 있습니다.<a href="#source-2">[2]</a>
여기서 JSON은 모델이 ‘JSON 언어로 생각한다’는 증거가 아니라, 관련 정보를 이름 붙여 전달하는 그릇으로 이해하면 됩니다.<a href="#source-2">[2]</a>

예를 들어 합성 사례의 입력을 `message`, `verified_transactions`, `refund_policy`로 나누어 담을 수 있습니다. 이는 설명을 위한 필자의 구성이지 실제 API 요청 전문이나 실행 결과가 아닙니다. 중요한 점은 고객이 주장한 중복 결제와 시스템에서 확인한 거래 기록을 같은 사실로 취급하지 않는 것입니다.

공식 문서는 판단 재료인 State와 판단할 내용을 지정하는 Questions를 구분합니다.<a href="#source-2">[2]</a>
한 요청의 질문들은 같은 State를 보고 독립적으로 평가됩니다.<a href="#source-2">[2]</a>
재료를 주는 일과 무엇을 물을지 정하는 일은 별개의 인터페이스입니다.<a href="#source-2">[2]</a>
어떤 질문을 함께 묶고 어떤 질문을 나눌지는 3편에서 다룹니다.

<figure>
  <img src="/assets/img/posts/jev-structure-vs-llm/01-public-contract.png" alt="고객 문의와 확인된 기록을 담은 State, 별도로 정한 Questions를 Jev에 전달하고 타입 있는 판단을 코드가 받는 공개 입출력 개념도" width="720" height="1160" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 1. 공개 문서를 바탕으로 새로 그린 입력·출력 계약입니다. 연결선은 사용 순서이며 내부 신경망의 층이나 실제 실행 화면이 아닙니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
그림: Clark, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-structure-vs-llm/01-public-contract.png">원본 이미지 보기</a></figcaption>
</figure>

## 2. 돌려받는 것은 ‘잘 쓴 설명’이 아닙니다

문서의 기본 질문 유형은 Choice·Score·Noul입니다.<a href="#source-3">[3]</a>
Choice는 선택 결과와 선택지별 확률, confidence를, Score는 정의한 단계 위의 점수와 단계 정보·확률·confidence를 반환합니다.<a href="#source-3">[3]</a>
Noul은 명제가 참일 확률을 반환하며 별도 confidence는 없습니다.<a href="#source-3">[3]</a>

이름보다 중요한 것은 **어떤 종류의 값을 받을지 먼저 정한다는 점**입니다.<a href="#source-3">[3]</a>
문의의 담당 팀을 고르는 일, 불만 정도를 척도에 놓는 일, 환불을 요청했는지 판단하는 일은 서로 다른 출력 계약입니다.<a href="#source-3">[3]</a>
이번 편에서는 각 확률의 해석이나 임계값을 깊게 다루지 않습니다. confidence와 정답 가능성의 차이는 4편의 주제입니다.

공식 문서상 Jev는 답장·코드·추론 설명을 생성하지 않습니다.<a href="#source-1">[1]</a>
따라서 고객에게 보낼 사과문까지 Jev에서 받아내려는 것은 현재 제품의 출력 범위와 맞지 않습니다.<a href="#source-1">[1]</a>
여기서는 판단 결과를 코드가 읽고, 필요하다면 별도 템플릿이나 생성 모델이 고객용 설명을 맡는 역할 분담을 생각할 수 있습니다. 이는 제품의 숨은 기능이 아니라 필자의 시스템 구성 예시입니다.

## 3. LLM도 구조화된 값을 반환합니다

**‘LLM은 문장만, Jev는 JSON만’이라는 대비는 정확하지 않습니다.**<a href="#source-1">[1]</a><a href="#source-6">[6]</a>
OpenAI의 현재 문서는 Structured Outputs로 개발자가 지정한 JSON Schema에 맞는 응답을 생성할 수 있다고 설명합니다.<a href="#source-6">[6]</a>
JSON 문법을 맞추는 JSON mode와 스키마 준수를 목표로 하는 Structured Outputs도 구분합니다.<a href="#source-6">[6]</a>

따라서 기존 LLM으로도 담당 팀을 허용된 목록에서 고르도록 하고, 필요한 필드를 가진 구조화된 응답을 받을 수 있습니다.<a href="#source-6">[6]</a>
Jev만 코드가 읽을 수 있는 값을 반환할 수 있다는 주장은 성립하지 않습니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>
‘LLM이면 언제나 자유 문장을 파싱해야 한다’는 가정 역시 비교의 출발점으로 삼지 않아야 합니다.<a href="#source-6">[6]</a>

이 문서 비교에 따른 필자의 해석은, **출력 형식의 독점성보다 제품이 무엇을 기본 계약으로 삼는가**를 살펴봐야 한다는 것입니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>
Jev는 질문 유형과 판단·확률을 중심에 놓습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>
LLM Structured Outputs는 지원하는 스키마 안에서 생성 응답을 구조화하는 기능입니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-6">[6]</a>
같은 분류 작업을 표현할 수 있어도 설정 방법과 반환 필드가 똑같다고 볼 수는 없습니다.<a href="#source-3">[3]</a><a href="#source-6">[6]</a>

또한 스키마가 올바르다는 사실과 선택한 담당 팀이 옳다는 사실은 다릅니다.<a href="#source-6">[6]</a>
OpenAI도 Structured Outputs가 내용상의 실수를 없애지는 않는다고 명시합니다.<a href="#source-6">[6]</a>
여기서 필요한 비교는 ‘JSON을 누가 더 예쁘게 쓰는가’가 아니라, 우리 코드가 요구하는 판단과 예외 처리를 어떻게 표현하고 검사할 수 있는가입니다.

<figure>
  <img src="/assets/img/posts/jev-structure-vs-llm/02-structured-outputs.png" alt="LLM의 Structured Outputs와 Jev의 판단 출력을 각각 카드로 설명하고 양쪽 모두 구조화된 값을 반환할 수 있으며 형식과 내용 정확도는 다르다고 표시한 비교도" width="720" height="1160" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 2. 서로 다른 제품의 출력 계약을 비교한 직접 제작 설명도입니다. 두 모델의 정확도·속도 순위나 실제 응답을 나타내지 않습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-6">[6]</a>
그림: Clark, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-structure-vs-llm/02-structured-outputs.png">원본 이미지 보기</a></figcaption>
</figure>

## 4. 병렬 출력이라는 설명은 어디까지 확인됐을까요?

TypeSafe의 소개글은 Jev가 문자열을 토큰 단위로 차례로 생성하는 대신 확률들을 병렬로 출력한다고 설명합니다.<a href="#source-5">[5]</a>
질문 문서 역시 한 요청 안의 질문들을 병렬 평가한다고 밝힙니다.<a href="#source-5">[5]</a><a href="#source-3">[3]</a>
필자는 두 설명을 각각 출력 방식과 여러 질문의 처리 계약에 관한 설명으로 읽으며, 이를 내부 구현 전체의 공개로 해석하지 않습니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>

이를 읽고 ‘질문마다 독립된 작은 신경망이 하나씩 있다’거나 ‘특정한 encoder 구조를 쓴다’고 그리면 문서보다 앞서 나가게 됩니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>
회사는 새 모델 아키텍처와 병렬 sampler를 개발했다고 설명하지만, 이번에 읽은 소개글과 문서만으로 구체적인 층 구성이나 파라미터 수, 학습 데이터와 실행 엔진을 재구성할 수는 없습니다.<a href="#source-5">[5]</a>

따라서 이 글에서는 **공개 인터페이스**, **회사의 처리 방식 설명**, **현재 자료로 확인하지 못한 구현**을 다른 칸에 둡니다. 병렬이라는 단어만으로 입력 처리·통신·후속 코드까지 모두 병렬이라거나, 질문을 늘려도 비용과 지연이 전혀 늘지 않는다고 해석하지 않습니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>
어떤 조건에서 얼마나 유리한지는 별도 증거의 문제이며 6편에서 검토합니다.

## 5. RLCD는 학습 목표이지 공개 설계도는 아닙니다

회사가 공개한 AI primer는 사전학습 언어모델 이후의 학습 경로를 설명하면서, 사람이 선호하는 응답을 지향하는 RLHF, 검증 가능한 보상을 이용하는 RLVR, 판단과 보정된 확률을 목표로 하는 RLCD를 대비합니다.<a href="#source-4">[4]</a>
TypeSafe는 자기 학습 경로를 RLCD라고 부릅니다.<a href="#source-4">[4]</a>
이 도식은 회사가 제시한 비교 틀로만 사용하며, 모든 LLM의 학습을 포괄하는 분류라고 해석하지 않습니다.<a href="#source-4">[4]</a>

RLCD는 **Reinforcement Learning for Calibrated Decisions**, 즉 보정된 판단을 위한 강화학습이라는 회사의 명칭입니다.<a href="#source-4">[4]</a><a href="#source-5">[5]</a>
공식 설명의 초점은 읽기 좋은 문장보다 판단과 확률을 반환하는 데 있습니다.<a href="#source-4">[4]</a>
어려운 질문에서도 늘 자신 있게 답하는 대신 불확실성을 소프트웨어가 사용할 수 있도록 만들겠다는 설계 방향으로 읽을 수 있습니다.<a href="#source-4">[4]</a>

다만 **그 목표를 밝혔다고 목표 달성까지 입증된 것은 아닙니다.** 회사 문서도 보정은 여러 예측의 집단에서 평가하는 성질이며 개별 답을 보장하지 않는다고 설명합니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a>
‘RLCD’라는 이름 하나로 보상 함수의 정확한 수식, 데이터 구성, 학습 과정이나 우리 업무에서의 확률 품질을 확인했다고 할 수는 없습니다.<a href="#source-4">[4]</a><a href="#source-5">[5]</a>
이번 자료는 목적을 이해하는 근거이지 구현 재현이나 독립 검증의 결과가 아닙니다.

<figure>
  <img src="/assets/img/posts/jev-structure-vs-llm/03-rlcd-boundary.png" alt="RLCD를 판단과 보정된 확률을 위한 회사의 공개 학습 목표로 표시하고 내부 층 구성과 보상 수식 등은 이번 자료만으로 재구성하지 않는다고 분리한 경계도" width="720" height="1160" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 3. 학습 목표의 설명과 구현을 안다는 주장을 구분한 직접 제작 개념도입니다. TypeSafe의 원본 그림을 복제하지 않았으며 RLCD 내부 알고리즘을 묘사하지 않습니다.<a href="#source-4">[4]</a><a href="#source-5">[5]</a>
그림: Clark, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-structure-vs-llm/03-rlcd-boundary.png">원본 이미지 보기</a></figcaption>
</figure>

## 6. 문의를 알아듣는 것과 환불하는 것은 다릅니다

처음의 문의를 결제 담당으로 보내고 환불 요청 여부를 기록하는 일은 Jev의 판단형 인터페이스로 표현할 수 있습니다.<a href="#source-1">[1]</a>
하지만 예시 속 고객의 말만으로 실제 중복 결제가 입증되지는 않습니다. 공식 환불 예시도 모델의 답을 결정적 코드 검사와 조합한 뒤 처리 또는 검토로 보내는 단계를 둡니다.<a href="#source-1">[1]</a>

반대로 “결제 내역이 이상해요. 한번 봐주세요”라는 메시지에는 명시적인 환불 요청이나 중복 결제 정보가 부족합니다. 이때 필요한 것은 확정적인 환불 결론을 꾸미는 일이 아니라, 무엇이 불명확한지 남기는 설계입니다. 두 문장은 합성 예시이며 어떤 모델에 입력해 얻은 결과도 아닙니다.

독자가 기억할 차이는 이것입니다. Jev는 제한된 판단과 확률을 중심에 둔 제품이고, 기존 LLM도 구조화된 출력을 제공할 수 있습니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>
둘의 차이는 JSON 소유권이 아니라 **기본 출력 계약과 회사가 내세운 처리·학습 목표**에서 살펴봐야 합니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a><a href="#source-6">[6]</a>
무엇을 채택하든 판단 결과를 실행 권한으로 바꾸는 규칙은 서비스가 책임져야 합니다.

<figure>
  <img src="/assets/img/posts/jev-structure-vs-llm/04-two-stamps.png" alt="직접 그린 작은 로봇이 환불 요청을 찾았다는 메모를 들고 있지만 실제 환불 승인 도장은 별도 금고 안에 있는 가벼운 삽화" width="720" height="960" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 4. ‘요청은 찾았지만 승인 도장은 따로’라는 필자의 역할 분담 삽화입니다. 실제 서비스 화면·모델의 판단 결과가 아닙니다. 그림: Clark, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-structure-vs-llm/04-two-stamps.png">원본 이미지 보기</a></figcaption>
</figure>

다음 3편에서는 이 인터페이스에 넣을 질문을 설계합니다. ‘알아서 처리해줘’를 어떤 작은 판단으로 바꿀지, 충분한 정보가 없는 문의를 어떻게 남길지 살펴봅니다. 확률과 신뢰성은 4편, 공개 성능 주장의 근거는 6편으로 이어집니다.

---

자료 확인일: 2026년 11월 2일(KST). 공개 문서와 제공자 설명을 읽은 해설입니다. API 호출·유료 서비스 이용·모델 성능 실험·독립 재현은 수행하지 않았습니다. 문서의 현재 인터페이스와 제공자의 설계 주장을 구분하며, 성능 우열을 확정하지 않습니다.

<nav id="jev-series-bottom" aria-label="JEV 탐구 연재 bottom">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><strong aria-current="page">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</strong></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><a href="/posts/jev-strengths-limits-trust/">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</a></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><a href="/posts/jev-adoption-system-design/">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</a></li>
  </ol>
  <p><a href="/posts/jev-system-1-vs-system-2/">← 이전 편</a> · <a href="/posts/jev-designing-small-questions/">다음 편 →</a></p>
</nav>

## Sources

[1] <span id="source-1" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/concepts/system-one.md">TypeSafe · System One</a>

[2] <span id="source-2" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/concepts/state.md">TypeSafe · State</a>

[3] <span id="source-3" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/primitives.md">TypeSafe · Primitives (Questions)</a>

[4] <span id="source-4" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/introduction/machine-learning-primer.md">TypeSafe · AI primer</a>

[5] <span id="source-5" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://typesafe.ai/blog/introducing-system-one-models-and-jev">TypeSafe · Introducing System One Models &amp; Jev</a>

[6] <span id="source-6" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://developers.openai.com/api/docs/guides/structured-outputs">OpenAI · Structured model outputs</a>
