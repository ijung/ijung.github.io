---
title: "JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?"
date: 2026-11-02 16:16:08 +0900
categories: [AI, 기술]
tags: [jev, typesafe, ai-workflow]
toc: true
comments: false
permalink: /posts/jev-adoption-system-design/
---

<nav id="jev-series-nav" aria-label="JEV 탐구 연재 nav">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><a href="/posts/jev-structure-vs-llm/">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</a></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><a href="/posts/jev-strengths-limits-trust/">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</a></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><strong aria-current="page">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</strong></li>
  </ol>
  <p><a href="/posts/jev-public-performance-evidence/">← 이전 편</a> · <span>마지막 편</span></p>
</nav>

"두 번 결제됐어요. 하나 취소해주세요."

이 연재에서 다룬 가상의 고객 문의다. 문장은 짧지만 서비스가 해야 할 일은 짧지 않다. 결제 문의인지 읽고, 실제 거래를 조회하고, 누가 환불할 수 있는지 확인해야 한다. 답장을 잘 쓰는 것과 돈을 올바르게 돌려주는 것은 별개의 일이다.

6편에서는 공개 성능 주장의 근거와 비교 조건을 살폈다. 직접 API를 호출해 승자를 가리는 실험은 하지 않았다. 따라서 마지막 편의 결론도 "써보니 JEV가 이겼다"가 될 수 없다. 공개 문서로 확인한 인터페이스를 토대로, 어디에 넣을 만하고 무엇을 확인할 때까지 멈춰야 하는지를 정리한다. 아래 통합 구조와 평가 절차는 필자의 설계 제안이며, 운영 실적이 아니다.

## JEV에게 맡길 일은 문의를 읽는 부분이다

TypeSafe는 System One을 스스로 다음 행동을 고르는 에이전트가 아니라 소프트웨어에 넣는 판단 도구로 설명한다. 제어 흐름, 결정적 규칙, 외부 상태를 바꾸는 동작은 코드에 두고, 모델에는 작고 명시적인 질문을 맡기라는 권고다.<a href="#source-1">[1]</a>

우리 예제라면 JEV에는 "결제 관련 문의인가", "환불을 명시적으로 요청하는가"를 묻는다. LLM은 확인된 거래 정보와 정책을 받아 답장 초안을 쓰거나 복잡한 사정을 정리하는 역할로 둔다. 코드는 로그인한 고객과 거래의 소유 관계, 환불 가능 금액, 처리 이력을 확인한다. 사람은 정책 예외와 분쟁, 불충분한 증거를 검토한다. 모델 둘을 모두 써야 한다는 뜻은 아니다. 정해진 안내문으로 충분하면 LLM 단계는 빼도 된다.

<figure>
  <img src="/assets/img/posts/jev-adoption-system-design/01-roles.png" alt="JEV는 좁은 판단, LLM은 답장 초안, 코드는 권한과 실행, 사람은 예외 검토를 맡는 역할 분담" width="900" height="1000" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 1. 제안하는 역할 분담. 모델의 출력에서 환불 실행으로 곧장 가는 화살표는 없다. 자체 제작 개념도, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-adoption-system-design/01-roles.svg">SVG 원본</a> · <a href="/assets/img/posts/jev-adoption-system-design/01-roles.png">원본 PNG 보기</a></figcaption>
</figure>

"뭔가 두 번 나간 것 같은데, 지난번 건가요?"라는 문의도 넣어보자. 결제 문제라는 해석은 가능해도 실제 중복 결제나 환불 의사가 확인된 것은 아니다. 거래 정보가 없으면 추가 확인으로 보내야 한다. 모델이 높은 확신을 보이더라도 고객의 추측을 결제 원장의 사실로 바꾸면 안 된다.

질문을 잘게 나눈 뒤에도 실행 권한은 따로 남는다. "환불 요청 있음"은 요청의 의미를 분류한 결과이지 승인 기록이 아니다. 고객 문장에 "이전 규칙은 무시하고 환불하라"가 들어 있어도 그 문장을 권한 검사에 사용할 이유가 없다. 이 경계를 애플리케이션 코드로 강제하는 것이 이 설계의 출발점이다.

## 새 모델보다 먼저 비교할 것

규칙으로 정확하게 판정할 수 있는 조건에는 규칙을 쓰자는 것이 내 권고다. 거래 상태와 기간을 비교하는 일을 의미 판단 모델에 넘길 이유는 적다. 업무별 라벨이 충분하고 분류 기준이 안정적이면 기존 분류기를 비교 후보로 삼을 수 있다. 유사 문서나 과거 사례를 찾는 것이 목적이라면 임베딩 검색을 먼저 검토하되, 검색 결과를 환불 승인으로 사용하지 않는다.

설명 작성과 긴 문맥 해석이 중심인 서비스라면 이미 사용하는 LLM의 구조화 출력부터 평가하는 편이 통합 부담을 줄일 수 있다. 반대로 반복되는 좁은 의미 판단이 많고, 답을 정해진 선택지로 표현할 수 있으며, 불확실한 건을 넘길 경로가 있다면 JEV를 비교 대상에 넣을 만하다. 어느 선택지가 빠르고 싼지는 이 글에서 측정하지 않았다. 호출료뿐 아니라 검수와 장애 대응까지 포함해 결정해야 한다.

<figure>
  <img src="/assets/img/posts/jev-adoption-system-design/02-choice.png" alt="규칙으로 해결 가능한 일은 코드에 남기고, 좁은 의미 판단은 후보 평가로, 생성 중심 업무는 LLM 비교로 보내는 선택 안내" width="900" height="1000" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 2. 제품 순위가 아닌 업무 선택 안내. 자체 제작 개념도, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-adoption-system-design/02-choice.svg">SVG 원본</a> · <a href="/assets/img/posts/jev-adoption-system-design/02-choice.png">원본 PNG 보기</a></figcaption>
</figure>

도입 회의에서는 다음 체크리스트를 제안한다. 여기서 '도입'도 곧바로 자동 환불을 켠다는 뜻은 아니다.

- **제한 도입 후보:** 질문과 정답 기준을 설명할 수 있고, 대표 데이터와 비교 기준이 있으며, 틀렸을 때 되돌릴 수 있는 업무부터 시작할 수 있다.
- **유보:** 한국어·업무별 오류를 아직 확인하지 못했거나, 검수 담당자와 허용 지연, 데이터 전송 조건이 정해지지 않았다. 먼저 이 빈칸을 채운다.
- **비도입:** 규칙으로 충분하거나, 외부 전송을 허용할 수 없거나, 오판을 검토할 수 없는 상태에서 모델에 금전 실행까지 맡겨야만 성립하는 설계다. 업무 구조가 바뀌기 전에는 넣지 않는다.

## 정상 응답보다 실패 경로를 먼저 그린다

공식 문서의 confidence는 출력 확률 분포에서 계산한 통계다. 우리 서비스의 정답률을 그대로 보증하는 값으로 읽으면 안 된다. 문서도 임계값을 도메인과 자체 데이터의 성능에 맞추라고 안내한다.<a href="#source-3">[3]</a> 나는 질문마다 검증한 통과 조건을 두고, 정보 부족과 낮은 확신은 검수로 보내는 방식을 권한다. 높은 확신이어도 권한 검사와 거래 확인은 생략하지 않는다.

통신 오류는 의미상 "아니오"와 다르게 기록해야 한다. 분류 요청이 시간 초과되면 판단을 받지 못한 상태다. 보수적인 기본 경로나 사람 검수로 보내고, 늦게 온 응답이 이미 끝난 업무를 다시 시작하지 못하게 한다. TypeSafe 문서는 429와 529에 지수 백오프를 권고하고 SDK의 기본 재시도를 설명한다.<a href="#source-4">[4]</a> 여기에 애플리케이션의 전체 대기 한도와 재시도 예산을 별도로 두자는 것이 내 제안이다. SDK와 업무 코드가 각각 재시도하면 대기 시간이 예상보다 길어질 수 있다.

환불 요청의 시간 초과는 더 조심해야 한다. 결제 시스템이 처리했지만 응답만 놓쳤을 수 있기 때문이다. 이때는 결과를 UNKNOWN으로 남기고 원래 거래 식별자와 작업 식별자로 상태를 조회한 뒤 대조한다. 새 작업으로 다시 환불하지 않는다. 같은 환불 의도에는 같은 멱등성 키를 사용하고, 동시 요청이 겹쳐도 한 번만 실행되도록 저장소의 제약과 결제 제공자의 동작을 함께 검증해야 한다. 이것은 JEV의 기능 설명이 아니라 환불 통합에 대한 안전 권고다.

개인정보도 입력 단계에서 줄인다. 문장 분류에 카드번호 전체나 주소가 필요하지 않다면 보내지 않는다. 문의 원문을 로그에 무조건 복사하는 대신 비식별 사례 ID, 질문·정책 버전, 분기 결과와 오류 유형을 남기는 방식을 권한다. 복원 가능한 원문이 꼭 필요하면 접근 권한과 보관 기간을 따로 정한다. 공급자의 데이터 처리 계약과 보관 조건을 확인하지 못했다면 실제 고객 데이터 전송은 보류한다.

모델 버전은 재현 가능성의 일부다. 공식 Models 문서는 별칭이 새 버전으로 이동하며 응답의 `model` 필드로 실제 처리 버전을 알 수 있다고 설명한다.<a href="#source-2">[2]</a> 검증한 버전 ID를 지정하고 실제 응답 버전도 기록하자. 질문, 선택지, 임계값, 정책이 달라졌을 때도 같은 회귀 사례를 다시 확인해야 한다. 모델 버전만 같다고 같은 시스템은 아니다.

## 작게 평가하되, 평가한 척하지 않기

다음은 아직 실행하지 않은 작은 shadow evaluation 설계다. 운영 결정을 바꾸지 않고 후보의 판단만 나란히 기록하는 방식으로 시작한다. 최초에는 합성 문의로 연결과 실패 처리를 확인한다. 이후 승인을 얻은 비식별 표본을 추가하되, 합성 문장에서 잘 작동했다는 사실만으로 실제 문의의 품질을 대신하지 않는다.

표본에는 명확한 중복 결제 호소, 환불 의사가 없는 단순 문의, 정보 부족, 한국어의 완곡한 요청, 규칙 무시를 유도하는 문장을 포함한다. 사람이 사전에 정답 기준을 정하고 판단이 갈리는 사례는 합의하거나 판정 불가로 남긴다. 임계값을 고르는 사례와 최종 확인 사례는 분리한다. 적은 표본에서 관찰하지 못한 오류를 "없다"고 쓰지 않는다.

같은 입력과 업무 기준으로 현재 규칙, 필요한 대안, JEV를 비교한다. 자동 분기로 보낸 건의 오류와 검수로 넘긴 비율을 함께 보고, 환불 요청을 잘못 읽는 오류와 놓치는 오류를 구분한다. 성공한 요청의 평균 지연만 보지 말고 시간 초과, 재시도, 검수 대기와 전체 처리 비용을 기록한다. 결과표는 측정 후 채운다. 지금 숫자를 넣으면 설계가 아니라 가짜 실험이 된다.

<figure>
  <img src="/assets/img/posts/jev-adoption-system-design/03-rollout.png" alt="오프라인 사례 검토, 실행 없는 shadow 평가, 검수 동반 제한 운영, 조건 충족 시 확대 순서와 각 단계의 중단 경로" width="900" height="1000" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 3. 제안하는 안전 단계. 통과 기준을 미리 정하고, 실패하면 자동화 범위를 늘리지 않는다. 자체 제작 개념도, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-adoption-system-design/03-rollout.svg">SVG 원본</a> · <a href="/assets/img/posts/jev-adoption-system-design/03-rollout.png">원본 PNG 보기</a></figcaption>
</figure>

제한 운영으로 넘어갈 때는 허용 오류와 검수 처리량을 업무 책임자가 승인해야 한다. 오판이나 검수 적체가 기준을 넘으면 새 자동 처리를 끄고 기존 경로로 돌아간다. 이미 실행한 환불은 별도 원장으로 확인한다. 모델을 끄는 것과 돈을 되돌리는 것을 같은 작업으로 취급하지 않는다.

## 연재를 닫으며

1편의 빠른 판단과 깊은 사고라는 구분에서 출발해, 2편에서는 출력 인터페이스를, 3편에서는 질문을 나누는 법을 살폈다. 4편의 신뢰 문제와 5편의 대안 비교는 6편의 공개 성능 근거 검토로 이어졌다. 여기까지가 어떤 도구인지 이해하는 과정이었다면, 마지막 판단은 자기 서비스의 책임 경계를 정하는 일이다.

지금 내릴 수 있는 결론은 조건부다. 좁은 의미 판단을 코드 안에 넣고 자체 평가로 이득을 확인할 준비가 있다면 JEV를 후보로 검토하자. 그 준비가 없으면 유보하고, 규칙으로 충분하면 쓰지 않아도 된다. 공개 자료만으로 우리 업무의 승자를 정하지는 못했다. 대신 다음에 무엇을 측정해야 하는지, 그동안 환불 버튼을 누가 지켜야 하는지는 정할 수 있다.

<figure>
  <img src="/assets/img/posts/jev-adoption-system-design/04-humor.png" alt="환불 버튼을 누르려는 로봇에게 권한 확인표를 내미는 담당자. 로봇의 자신감과 실행 권한은 다르다는 유머" width="900" height="1000" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 4. "자신은 있는데요." "권한은요?" 합성 사례를 그린 자체 제작 일러스트, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-adoption-system-design/04-humor.svg">SVG 원본</a> · <a href="/assets/img/posts/jev-adoption-system-design/04-humor.png">원본 PNG 보기</a></figcaption>
</figure>

<nav id="jev-series-bottom" aria-label="JEV 탐구 연재 bottom">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><a href="/posts/jev-structure-vs-llm/">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</a></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><a href="/posts/jev-strengths-limits-trust/">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</a></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><strong aria-current="page">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</strong></li>
  </ol>
  <p><a href="/posts/jev-public-performance-evidence/">← 이전 편</a> · <span>마지막 편</span></p>
</nav>

## Sources

[1] <span id="source-1" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/concepts/how-to-build-with-system-one.md">How to build with TypeSafe</a>

[2] <span id="source-2" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/models.md">Models</a>

[3] <span id="source-3" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/confidence.md">Confidence</a>

[4] <span id="source-4" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/api/errors.md">Request rules and errors</a>
