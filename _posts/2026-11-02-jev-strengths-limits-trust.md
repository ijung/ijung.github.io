---
title: "JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?"
date: 2026-11-02 16:16:08 +0900
categories: [AI, 기술]
tags: [jev, typesafe, ai-workflow]
toc: true
comments: false
permalink: /posts/jev-strengths-limits-trust/
---
<p><strong>예상 읽기: 약 9분 · 난이도: 중급</strong> — 출력 계약과 확률 개념을 다룹니다.</p>


<nav id="jev-series-nav" aria-label="JEV 탐구 연재 nav">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><a href="/posts/jev-structure-vs-llm/">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</a></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><strong aria-current="page">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</strong></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><a href="/posts/jev-adoption-system-design/">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</a></li>
  </ol>
  <p><a href="/posts/jev-designing-small-questions/">← 이전 편</a> · <a href="/posts/jev-alternatives-comparison/">다음 편 →</a></p>
</nav>

고객이 "같은 주문이 두 번 결제됐어요. 하나는 취소해 주세요"라고 썼다. 문의를 결제 담당에게 보내는 판단과 실제로 돈을 돌려주는 판단 사이에는 꽤 긴 거리가 있다. 앞의 일에는 문장의 뜻을 파악하는 능력이 필요하다. 뒤의 일에는 실제 거래 기록, 이미 처리한 환불, 고객의 권한과 회사 정책이 필요하다. 이 글의 고객 문의는 설명을 위해 만든 가상 사례다. JEV에 호출해 얻은 결과나 실제 고객 기록이 아니다.

JEV를 살펴보면서 눈여겨볼 강점은 바로 앞쪽이다. TypeSafe 문서에 따르면 JEV는 자연어를 읽고, 자유로운 답변 대신 정해진 형태의 판단과 확률을 반환한다. 답장을 쓰거나 판단 이유를 길게 설명하는 모델은 아니다.<a href="#source-1">[1]</a> 따라서 "이 고객을 알아서 도와줘"보다 "어느 팀이 맡아야 하는가", "환불을 명시적으로 요청했는가" 같은 질문이 인터페이스에 잘 맞는다.<a href="#source-5">[5]</a> 판단 결과를 코드에서 다루기 쉽다는 장점은 분명하다. 다만 그 편리함이 정답 보증으로 바뀌는 순간부터 사고가 시작된다.

## JSON이 멀쩡해도 판단은 틀릴 수 있다

결제 문의에 `billing`이라는 값이 왔다고 가정하자. 반환값이 허용된 선택지에 속한다면 코드는 문자열을 다시 해석할 필요 없이 담당팀으로 보낼 수 있다. Choice가 정해진 선택지와 각 선택지의 확률을 반환한다는 것은 문서로 확인되는 기능이다.<a href="#source-8">[8]</a> 그러나 같은 문장을 보고 `shipping`을 골랐더라도 형식은 여전히 정상일 수 있다. 스키마 검증은 "배송팀이라는 값이 허용되는가"를 검사할 뿐, "이 고객을 배송팀으로 보내는 것이 맞는가"를 판정하지 않는다.

<figure>
  <img src="/assets/img/posts/jev-strengths-limits-trust/01-format-meaning.png" alt="허용되지 않는 값은 형식 검사에서 걸리지만, 허용된 오답은 의미 검토가 필요하다는 비교" width="1200" height="800" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 1. 형식 오류와 의미 오류는 다른 검사 대상이다. 가상 사례를 그린 자체 제작 개념도. <a href="/assets/img/posts/jev-strengths-limits-trust/01-format-meaning.svg">원본 SVG</a> · <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. · <a href="/assets/img/posts/jev-strengths-limits-trust/01-format-meaning.png">원본 PNG 보기</a></figcaption>
</figure>

이 구분은 JEV를 폄하하려는 말이 아니다. 출력 형태가 제한되면 연결 코드를 단순하게 만들 수 있다. 다만 개발자는 파서 오류를 줄인 성과와 업무 오판을 줄인 성과를 따로 기록해야 한다. 형식 검사 통과율만 보고 "신뢰할 수 있다"고 결론 내리면 가장 중요한 오류가 통계 밖으로 빠진다. 결제팀 분류의 정답은 담당 조직이 붙인 검증 라벨로, 중복 청구 여부는 거래 원장으로 확인하는 식으로 판단마다 근거를 달리 잡아야 한다.

## 확률, confidence, Score는 서로 바꿔 읽으면 안 된다

Noul은 예·아니오 질문에서 '예'일 확률을 반환한다.<a href="#source-6">[6]</a> 예를 들어 "환불을 요청했는가"에 대한 값이 중간에 있다면, 그것은 환불 의향의 세기가 중간이라는 뜻이 아니다. 질문에 예라고 답할지 불확실하다는 뜻이다. 의향의 강도나 불만의 정도를 보고 싶다면 수준을 정의한 Score가 더 맞는다.<a href="#source-5">[5]</a>

현재 문서에서 Choice와 Score에는 `probabilities`와 별도의 `confidence`가 있지만, Noul에는 별도 `confidence`가 없다.<a href="#source-3">[3]</a> Noul용 confidence 필드를 가정해 공통 코드를 짜면 문서와 어긋난다. 필요하면 애플리케이션이 확률로부터 별도 지표를 계산할 수 있지만, 그 값은 반환 필드가 아니라 우리가 만든 파생값이라고 표시해야 한다.

여기서 `confidence`도 "이 답이 실제로 맞을 확률"이라는 별도의 인증서는 아니다. TypeSafe는 반환된 확률 분포로부터 이 값을 계산한다. Choice의 경우 선택지가 세 개이고 가장 큰 확률이 0.6이면 문서의 공식으로 계산한 confidence는 0.4다.<a href="#source-3">[3]</a> 이 숫자는 공식 설명을 위한 계산 예시이며 API 측정값이 아니다. 선택지 수에 따라 계산 기준이 달라지므로 서로 다른 질문에 같은 임계값을 붙이는 것도 신중해야 한다.

Score의 숫자는 또 다르다. 수준 번호를 확률로 가중평균한 위치다.<a href="#source-7">[7]</a> 가령 수준이 0·1·2일 때 전부 1에 몰린 분포와 0·2에 반씩 나뉜 분포는 모두 평균 1을 만든다. 전자는 가운데 수준이라는 판단이 뚜렷하고 후자는 양끝 사이에서 갈린다. 평균만 저장하면 이 차이를 잃는다. 높은 불만 Score를 높은 확신으로 읽거나, 중간 Score를 애매한 판단으로 읽어서는 안 된다.

<figure>
  <img src="/assets/img/posts/jev-strengths-limits-trust/02-three-numbers.png" alt="Noul의 예일 확률, Choice의 분포 요약인 confidence, Score의 수준 위치를 구분하는 개념도" width="1200" height="800" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 2. 비슷한 숫자라도 질문이 다르다. 데이터나 신뢰도 실측 그래프가 아닌 자체 제작 개념도. <a href="/assets/img/posts/jev-strengths-limits-trust/02-three-numbers.svg">원본 SVG</a> · <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. · <a href="/assets/img/posts/jev-strengths-limits-trust/02-three-numbers.png">원본 PNG 보기</a></figcaption>
</figure>

## 보정이 잘됐다는 말은 여러 건을 모아 확인한다

Calibration, 즉 확률 보정은 예측 확률과 실제 결과가 얼마나 맞아떨어지는지에 관한 문제다. Guo 등의 논문은 분류 모델에서 실제 정답 가능성을 대표하는 확률 추정치를 다루며, 조사한 현대 신경망들이 잘 보정되지 않는 문제를 보고했다.<a href="#source-2">[2]</a> 이 논문은 JEV 평가 논문이 아니다. JEV가 나쁘거나 좋다는 근거로 가져올 수는 없고, 확률이라는 이름만으로 신뢰성을 보장할 수 없는 이유를 이해하는 자료다.

TypeSafe는 System One 모델을 결과에 맞춰 확률을 최적화하는, 보정된 판단을 위한 모델이라고 설명한다. 같은 페이지에는 보정이 예측 집단에 대해 측정되며 개별 답의 정확성을 보장하지 않는다고 적혀 있다.<a href="#source-1">[1]</a> 앞 문장은 회사가 설명하는 학습 목표이고, 뒤 문장은 제품을 사용할 때 지켜야 할 해석의 한계다. 이 글에서는 한국어 결제 문의에 대한 독립 실측 자료를 확보하지 않았으므로 우리 사례에서 얼마나 보정됐는지는 확인하지 못했다.

실무에서는 정답을 확인할 수 있는 과거 문의를 따로 남겨 두고, 비슷한 확률을 받은 사례들을 모아 예측과 정답 빈도를 비교해야 한다. 단순히 "정확도가 높았다"만으로 끝내지 말고, 잘못 자동 처리한 건과 검토로 보낸 건을 나누어 볼 필요가 있다. 문의 전체에서 그럴듯해 보여도 한국어의 완곡한 요청이나 드문 이중 결제 사례에서 어긋날 수 있기 때문이다. 여기서는 측정 데이터가 없어서 신뢰도 곡선을 그리지 않았다. 보기 좋은 대각선은 검증 기록을 대신하지 못한다.

### 유창한 말투, 자기보고 숫자, 토큰 확률은 다르다

“확실합니다”라는 말투는 언어적 자신감 표현이고, “정답일 확률 90%”는 수치로 생성한 자기보고입니다. 모델이 다음 토큰에 부여하는 조건부 확률은 또 다른 값입니다. 이 셋을 곧바로 ‘업무 정답률’로 바꾸어 읽으면 안 됩니다.<a href="#source-9">[9]</a>
Tian 등의 2023년 연구는 당시 ChatGPT·GPT-4·Claude와 TriviaQA·SciQ·TruthfulQA에서 자기보고 방식과 조건부 확률의 보정 품질을 비교했습니다. 자기보고가 더 나은 결과도 있었으므로 ‘모든 LLM은 필연적으로 과신한다’는 일반화 역시 피해야 합니다.<a href="#source-9">[9]</a>
Kadavath 등의 연구도 적절한 형식의 선택형·참거짓 질문에서 보정 가능성을 보고했지만, 새로운 과업으로 옮길 때의 어려움을 함께 다룹니다. 문장이 유창하거나 숫자를 출력할 수 있다는 사실만으로 새 업무의 신뢰성을 확정할 수는 없습니다.<a href="#source-10">[10]</a>

### 같은 90%라도 실제 결과를 모아 보아야 한다

**다음은 서로 다른 세 가상 평가 상황입니다. 모델 호출·관측 데이터·성능 실측이 아닙니다.** 각 상황에서 모델이 선택한 답 100개에 모두 정답 가능성 90%를 붙였다고 가정합니다.

- 실제로 90개가 맞았다면, 이 집단에서는 예측 90%와 관측 정답률 90%가 일치합니다.
- 60개만 맞았다면, 이 집단에서는 예측이 실제 정답률보다 높아 **과신**한 셈입니다.
- 99개가 맞았다면, 이 집단에서는 예측이 실제 정답률보다 낮아 **과소신**한 셈입니다.

이는 보정의 뜻을 설명하는 산술 예시입니다. 개별 답의 성공 보증도, 표본 100개로 전체 보정을 증명했다는 주장도 아닙니다. Guo 등의 정의 역시 예측 확률을 조건으로 한 집단의 정답 가능성을 다룹니다.<a href="#source-2">[2]</a>

<figure>
  <img src="/assets/img/posts/jev-strengths-limits-trust/05-calibration-groups.png" alt="서로 다른 가상 상황에서 정답 가능성 90%를 붙인 100건 중 90건, 60건, 99건이 맞은 경우를 비교하며 실측이 아니라고 명시한 보정 설명도" width="900" height="1200" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>추가 그림. <strong>가상 산술 예시 — 실측 결과가 아닙니다.</strong> 각 행은 서로 다른 평가 상황이며 합쳐서 한 모델의 성능표로 읽지 않습니다. 개별 답이나 전체 분포의 보정을 보장하지 않습니다. 개념 근거는 Guo 등입니다.<a href="#source-2">[2]</a> 그림: Clark, <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. <a href="/assets/img/posts/jev-strengths-limits-trust/05-calibration-groups.svg">SVG 원본</a> · <a href="/assets/img/posts/jev-strengths-limits-trust/05-calibration-groups.png">원본 PNG 보기</a></figcaption>
</figure>

**정확도는 얼마나 맞혔는지, 보정은 붙인 확률이 관측 빈도와 얼마나 맞는지**를 묻습니다. 둘은 같은 지표가 아닙니다. Guo 등의 연구에서도 더 정확한 모델의 확신이 실제 정확도와 잘 맞지 않는 사례가 나옵니다.<a href="#source-2">[2]</a>

### ECE와 Brier도 한 줄 성적표로 끝내지 않는다

Guo 등의 ECE는 예측을 확률 구간(bin)으로 나누고, 각 구간의 **평균 예측 확률과 관측 정답률 차이의 절댓값**을 표본 비중으로 가중평균합니다. `ECE = Σ (구간 표본 수 / 전체 표본 수) × |구간 정답률 − 구간 평균 확률|`입니다.<a href="#source-2">[2]</a>
구간 수·경계와 표본 수를 같이 적어야 해석할 수 있으며, 한 구간의 평균이 맞더라도 그 안의 특정 고객군까지 안전하다는 뜻은 아닙니다. 낮은 ECE만으로 오답을 잘 가려내는 능력까지 보장하지도 않습니다.<a href="#source-2">[2]</a><a href="#source-9">[9]</a>

이진 사건의 Brier score는 `평균[(p − y)²]`로, 예측 확률 `p`와 실제 결과 `y∈{0,1}` 사이의 제곱 오차 평균입니다. 여기서는 이진형 정의를 쓰며, 다중분류의 클래스 합산·정규화와 섞어 비교하지 않습니다.<a href="#source-11">[11]</a>
Brier는 확률 예측의 전체 품질 지표이지 보정 오차만 분리한 값이 아닙니다. 보정뿐 아니라 사례를 구분하는 능력과 사건 빈도도 함께 보아야 합니다.<a href="#source-13">[13]</a>

### 숫자가 자동화의 분기 조건이 될 때

필자의 설계상 주의점은 과신을 잘못된 자동 실행으로 연결하지 않는 것입니다. “90% 이상이면 자동 환불”을 검증 없이 적용하면, 위의 가상 60% 상황에서는 생각보다 많은 오답이 통과합니다. 반대로 과소신은 불필요한 보류를 늘릴 수 있습니다. 먼저 분류·정보 조회와 환불 실행의 권한을 분리하고, 잘못 통과시킨 건수·검토량·손실을 함께 기록하는 편이 낫습니다.

TypeSafe는 RLCD를 **보정된 판단을 위한 강화학습 목표**로 설명하지만, 그 명칭이 우리 한국어 문의의 보정 완료를 뜻하지는 않습니다.<a href="#source-12">[12]</a>
또한 Jev의 `confidence`는 반환 분포에서 계산한 요약값이며, 실제 정답을 수집해 계산한 보정 지표가 아닙니다.<a href="#source-3">[3]</a>
필자는 정답 라벨이 있는 별도 평가 자료에서 업무·언어·모델 버전별로 정확도와 보정을 확인하고, 임계값을 맞춘 자료와 최종 평가 자료를 분리할 것을 권합니다. 입력 분포가 바뀌면 과거의 확률 해석을 그대로 유지할 근거도 다시 점검해야 합니다. 이는 새 과업에서의 보정 어려움과도 연결되는 한계입니다.<a href="#source-10">[10]</a>


## 한국어, 업무 용어, 긴 입력은 별도 조건이다

현재 Models 문서는 영어가 주된 학습 언어이고 정확도가 가장 좋으며, CJK를 포함한 다른 언어도 처리하지만 동등한 수준은 아니라고 명시한다.<a href="#source-4">[4]</a> 한국어를 입력할 수 있다는 것과 우리 상담 문장의 정확도가 확인됐다는 것은 다르다. "취소는 됐는데 아직 찍혀 있네요"처럼 결제 승인, 매입, 승인 취소가 섞이는 문장은 한국어 능력과 업무 지식 모두를 시험한다. 같은 문의를 영어로 번역해서 성공했다고 한국어 원문에도 성공했다고 기록해서는 안 된다.

문서상 고객별 파인튜닝이나 LoRA 적응은 제공하지 않으며, 같은 가중치에 `state`, 질문 지시문과 판단 기준으로 도메인 내용을 전달한다.<a href="#source-4">[4]</a> 따라서 사내 환불 규정과 예외를 요청에 잘 담는 작업이 중요하다. 그래도 자료가 빠졌거나 규정끼리 충돌하면 분명한 선택지를 만들어 주는 것만으로 해결되지 않는다. 이때의 대안은 자신 있게 답하도록 문구를 고치는 일이 아니라 필요한 사실을 더 조회하는 일이다.

긴 문서도 넣기만 하면 해결되는 조건은 아니다. 조회한 문서에서 Jev 1.14와 1.13의 기본 한도는 요청 전체 64k, 질문별 32k 토큰이다. 질문별 한도에도 state가 포함된다.<a href="#source-4">[4]</a> `TypeSafe-Danger-Zone-Long-Context: true` 헤더로 두 한도를 262k까지 늘릴 수 있지만 기본값은 꺼져 있고, 회사 문서 스스로 기본 한도 안에서 쓸 때보다 느리고 정확도가 낮다고 경고한다.<a href="#source-4">[4]</a> 긴 상담 이력 전체를 무조건 보내기보다 어떤 거래와 어떤 시점의 정책이 필요한지 먼저 좁히는 설계가 낫다는 것이 이 글의 판단이다.

## 애매한 고객에게는 추가 질문이 먼저다

이번에는 고객이 "또 결제된 것 같은데 지난번 건도 아직 보이네요. 확인 좀 해주세요"라고 썼다고 하자. 환불을 명시적으로 요청하지 않았고, 실제 두 번 청구됐는지도 알 수 없다. 단순히 청구 내역을 확인하려는 것일 수 있다. 이 문장에 예측 숫자를 붙여 JEV의 출력처럼 보여 줄 근거는 없다. 대신 애플리케이션이 해야 할 일은 분명하게 정할 수 있다. 관련 주문을 확인하고, 승인 대기와 확정 청구를 구분할 자료를 가져오며, 필요하면 고객에게 어떤 항목을 말하는지 묻는다.

확률이 애매할 때 보류하는 길은 유용하지만, 높은 confidence일 때도 입력 자료의 누락과 처리 권한은 별도로 확인해야 한다. 높은 값은 권한 확인을 생략하라는 지시가 아니다. 공식 문서도 임계값은 업무와 실제 성능에 따라 정하고 자체 데이터로 시험하라고 설명한다.<a href="#source-3">[3]</a> 문서 속 예시 임계값을 결제 시스템의 운영 기준으로 그대로 복사할 이유는 없다.

<figure>
  <img src="/assets/img/posts/jev-strengths-limits-trust/03-review-path.png" alt="입력 자료와 모델 판단을 확인한 뒤 정보 부족은 추가 질문으로, 낮은 확신은 검토로 보내고, 높은 확신도 권한과 정책 검사를 거치는 흐름" width="1200" height="800" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 3. 제안하는 검토 분기. 제품에 내장된 환불 정책이나 측정 결과가 아닌 설계 예시다. <a href="/assets/img/posts/jev-strengths-limits-trust/03-review-path.svg">원본 SVG</a> · <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. · <a href="/assets/img/posts/jev-strengths-limits-trust/03-review-path.png">원본 PNG 보기</a></figcaption>
</figure>

명확한 첫 번째 사례에서는 결제팀 분류까지 자동화할 여지가 있다. 환불 실행은 다른 경로에 둔다. 거래가 실제로 중복인지 코드와 원장으로 확인하고, 이미 환불한 거래가 아닌지 확인하며, 승인 권한을 가진 담당자나 별도로 검증된 정책 절차를 거친다. 사람에게 넘길 때도 "모델이 불확실합니다"만 보여 주면 검토가 어렵다. 원문, 관련 거래, 질문과 선택지, 반환 분포, 실제 모델 버전을 함께 보여 주는 편이 좋다.

<figure>
  <img src="/assets/img/posts/jev-strengths-limits-trust/04-confident-clerk.png" alt="모델이 자신 있게 환불이라고 말하지만 담당자가 확인하는 것은 자신감이 아니라 승인권한임을 보여 주는 만화" width="1200" height="800" loading="lazy" style="display:block;max-width:100%;height:auto;margin:auto;">
  <figcaption>그림 4. 자신감과 결재권은 다른 칸에 적는다. 가상 캐릭터로 만든 자체 제작 삽화. <a href="/assets/img/posts/jev-strengths-limits-trust/04-confident-clerk.svg">원본 SVG</a> · <a href="https://creativecommons.org/licenses/by/4.0/">CC BY 4.0</a>. · <a href="/assets/img/posts/jev-strengths-limits-trust/04-confident-clerk.png">원본 PNG 보기</a></figcaption>
</figure>

문서에서 `jev-latest`는 현재 `jev-1.14.0`을 가리키지만 별칭은 새 릴리스에 따라 바뀐다. TypeSafe는 임계값을 조정한 버전의 ID를 직접 지정하고 응답의 모델 ID를 기록하라고 안내한다.<a href="#source-4">[4]</a> 검증한 질문과 모델을 고정해야 나중에 오판이 늘었을 때 입력 변화인지 모델 변경인지 조사할 수 있다.

JEV의 강점은 좁은 자연어 판단을 코드가 쓰기 좋은 값으로 연결한다는 데 있다. 지원 범위의 제한도 뚜렷하다. 현재는 텍스트 입력만 받으며 이미지·음성·영상은 지원하지 않고, 설명문이나 답장을 생성하지 않는다.<a href="#source-1">[1]</a> 회사가 내세우는 빠른 판단과 보정 학습은 제품 설명으로 읽되, 우리 서비스의 지연 시간과 오판율은 별도로 확인해야 한다. 이 글은 유료 API를 호출하지 않았고 성능을 측정하지 않았다. 도입의 첫 단계로 추천할 수 있는 것은 환불 자동 실행이 아니라, 사람이 정답을 확인할 수 있는 문의 분류와 검토 보조다. 그 기록을 쌓은 뒤 자동 처리 범위를 넓혀도 늦지 않다.

<nav id="jev-series-bottom" aria-label="JEV 탐구 연재 bottom">
  <p><strong>JEV 탐구 · 7편 읽는 순서</strong></p>
  <ol>
    <li><a href="/posts/jev-system-1-vs-system-2/">JEV 탐구 ① — System 1 vs System 2: AI는 언제 깊이 생각해야 할까?</a></li>
    <li><a href="/posts/jev-structure-vs-llm/">JEV 탐구 ② — 문장 대신 판단을 반환하는 AI: JEV는 LLM과 무엇이 다를까?</a></li>
    <li><a href="/posts/jev-designing-small-questions/">JEV 탐구 ③ — ‘알아서 처리해줘’를 작은 질문으로 바꾸는 법</a></li>
    <li><strong aria-current="page">JEV 탐구 ④ — 빠른 판단은 어디까지 믿어도 될까?</strong></li>
    <li><a href="/posts/jev-alternatives-comparison/">JEV 탐구 ⑤ — 꼭 JEV여야 할까? 같은 문제를 푸는 다른 선택지들</a></li>
    <li><a href="/posts/jev-public-performance-evidence/">JEV 탐구 ⑥ — 정말 빠르고 경제적일까? 공개 성능 주장의 근거를 따져보다</a></li>
    <li><a href="/posts/jev-adoption-system-design/">JEV 탐구 ⑦ — 그래서, 우리 서비스에 JEV를 넣어야 할까?</a></li>
  </ol>
  <p><a href="/posts/jev-designing-small-questions/">← 이전 편</a> · <a href="/posts/jev-alternatives-comparison/">다음 편 →</a></p>
</nav>

보강 자료 확인: 2026년 11월 3일(KST). 논문·공식 공개 문서에 근거한 해설이며 새 모델 실측은 수행하지 않았습니다.

## Sources

[1] <span id="source-1" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/concepts/system-one.md">System One</a>

[2] <span id="source-2" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://proceedings.mlr.press/v70/guo17a.html">On Calibration of Modern Neural Networks</a>

[3] <span id="source-3" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/confidence.md">Confidence</a>

[4] <span id="source-4" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/models.md">Models</a>

[5] <span id="source-5" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/primitives.md">Primitives</a>

[6] <span id="source-6" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/primitives/noul.md">Noul</a>

[7] <span id="source-7" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/primitives/score.md">Score</a>

[8] <span id="source-8" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/primitives/choice.md">Choice</a>

[9] <span id="source-9" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://aclanthology.org/2023.emnlp-main.330">Tian et al. · Just Ask for Calibration</a>

[10] <span id="source-10" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://arxiv.org/html/2207.05221v4">Kadavath et al. · Language Models (Mostly) Know What They Know</a>

[11] <span id="source-11" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://scikit-learn.org/stable/modules/generated/sklearn.metrics.brier_score_loss.html">scikit-learn · brier_score_loss</a>

[12] <span id="source-12" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://docs.typesafe.ai/introduction/machine-learning-primer.md">TypeSafe · AI primer</a>

[13] <span id="source-13" style="display:inline-block;scroll-margin-top:6rem;"></span> <a href="https://scikit-learn.org/stable/modules/calibration.html">scikit-learn · Probability calibration</a>
