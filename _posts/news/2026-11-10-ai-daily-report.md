---
title: "AI 데일리 리포트 — 2026-11-10"
date: "2026-11-10 06:10:22 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-10/
---

검수 기준: 2026-11-10 06:10 KST

## 30초 브리핑

- Anthropic이 Claude Fable 5.2와 Mythos 5.2를 출시했으며, 두 모델의 접근 조건은 다릅니다.<a href="#source-1">[1]</a>
- LG AI연구원이 K-EXAONE 2.5를 서비스 개발·연구에 무료로 연결할 수 있는 API를 공개했습니다.<a href="#source-2">[2]</a>
- ChatGPT Enterprise를 기존 AWS 구매·청구 체계로 도입할 수 있게 됐습니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### Claude Fable·Mythos 5.2 출시, 성능 개선과 접근 조건을 함께 확인

**Anthropic이 어려운 기술 작업을 겨냥한 Fable 5.2와 Mythos 5.2를 출시하고 기본 API 단가를 낮췄습니다.**<a href="#source-1">[1]</a>

- **달라진 점:** 회사는 코딩·추론·시각 이해를 개선하고, 이전 Fable 5.1·Mythos 5.1보다 출력을 최대 30% 빠르게 생성한다고 밝혔습니다.<a href="#source-1">[1]</a> 이 수치는 회사 발표이며 전체 업무 완료 시간이 같은 비율로 줄어든다는 뜻은 아닙니다.
- **왜 중요한지:** 같은 기반 모델이어도 보호장치와 이용 심사에 따라 접근 범위가 달라집니다.<a href="#source-1">[1]</a> Fable 5.2는 AWS·Google Cloud·Azure 등에서 제공하지만, Mythos 5.2는 보안·생명과학 분야의 검증된 조직을 대상으로 합니다.<a href="#source-1">[1]</a>

<p id="def-verification">검증 프로그램은 보안·생명과학의 유익한 활용을 목적으로 하는 조직이 Mythos 접근을 신청하는 절차입니다.<a href="#source-1">[1]</a></p>

<p id="def-fast-mode">고속 모드는 출력 생성 속도를 높이는 선택형 옵션이며 기본 요금과 별도로 더 높은 단가가 적용됩니다.<a href="#source-1">[1]</a></p>

<p class="news-source"><a href="https://www.anthropic.com/claude-fable-and-mythos-5-2">Anthropic · Introducing Claude Fable 5.2 and Claude Mythos 5.2</a><br><span class="source-published">게시: 2026-11-09 · 시각 미공개</span></p>

### K-EXAONE 2.5, 기술 검증 모델의 무료 API 공개

**LG AI연구원이 8월 공개 버전에 추가 학습을 적용한 K-EXAONE 2.5를 무료 API로 제공한다고 발표했습니다.**<a href="#source-2">[2]</a>

<p id="def-api">이 발표의 API 이용은 EXAONE API Platform에서 키를 발급받아 서비스 개발·연구에 모델을 연결해 쓰는 방식입니다.<a href="#source-2">[2]</a></p>

- **달라진 점:** 개발자·스타트업·기업·연구기관뿐 아니라 누구나 이용할 수 있다고 안내했습니다.<a href="#source-2">[2]</a> 단순 성능 소개를 넘어 실제 개발에 쓸 수 있는 접점을 열었다는 의미가 있습니다.<a href="#source-2">[2]</a>
- **주의할 점:** 기술 검증용 모델이며 공개 데이터셋을 사용한 성능 비교도 LG의 내부 평가입니다.<a href="#source-2">[2]</a> 무료 제공의 종료 시점·요청량 제한·운영 보장 수준은 이번 발표문에서 확인하지 못했습니다.

<p class="news-source"><a href="https://www.lgresearch.ai/news/view?seq=755">LG AI Research · K-EXAONE 2.5 API Available for Free</a><br><span class="source-published">게시: 2026-11-09 · 시각 미공개</span></p>

### ChatGPT Enterprise, AWS Marketplace에서 구매 가능

**OpenAI가 ChatGPT Enterprise의 AWS Marketplace 판매를 시작했습니다.**<a href="#source-3">[3]</a>

- **달라진 점:** 기업이 기존 AWS 구매·청구 관계를 활용해 도입·관리하고, 적격 지출을 AWS 약정에 반영할 수 있습니다.<a href="#source-3">[3]</a> 직접 구매와 모델·기능은 같고 새 기능도 같은 시점에 제공한다고 설명했습니다.<a href="#source-3">[3]</a>
- **왜 중요한지:** 이번 변화는 새 모델 출시가 아니라 기업의 구매 경로 확장입니다.<a href="#source-3">[3]</a> AWS 약정 반영과 의료·금융 특화 상품에는 적격 조건이 있으므로 모든 고객에게 동일하게 적용되는 혜택으로 읽어서는 안 됩니다.<a href="#source-3">[3]</a>

<p class="news-source"><a href="https://openai.com/index/chatgpt-enterprise-aws-marketplace">OpenAI · ChatGPT Enterprise now available in AWS Marketplace</a><br><span class="source-published">게시: 2026-11-09 · 시각 미공개</span></p>

## AI 트렌드와 변화 신호

- **전날 대비 — 기억 연구에서 실제 이용 조건으로:** 전날 다룬 기억 활용·정정 사례와 별개로, 이번에는 모델 업데이트·무료 API·기업 구매 경로라는 실질 변화를 확인했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a>
- **관찰 키워드 — 접근 자격·요금·조달:** 모델 성능뿐 아니라 누가, 어떤 조건과 경로로 쓸 수 있는지가 이번 발표들을 읽는 공통 기준입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a> 이 사례들을 시장 전체의 도입률이나 관심도 상승으로 일반화하지 않습니다.

## 조사 한계

- 수집 마감은 2026-11-10 06:00 KST입니다. 최근 24시간을 우선하고 최대 72시간의 미보도 발표를 함께 살폈습니다. 채택한 원문은 모두 11월 9일 날짜를 표시하지만 정확한 게시 시각은 공개하지 않았습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a>
- 주요 기업 공식 목록과 일반 검색을 확인했지만 목록의 일부 항목·기간만 확인돼 연속적인 전체 범위 검증은 확보하지 못했습니다. 일부 보안 정책·투자·파트너십 후보는 원문 접근이나 교차 확인을 끝내지 못해 제외했습니다.
- 기업 발표를 기준으로 출시·제공 조건을 확인했습니다. 성능은 회사 자체 주장과 평가이며 독립적인 제품 시험 결과가 아닙니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a> 국내 계정별 제공 여부와 실제 구매·호출 동작은 시험하지 않았습니다.

## Sources

[1] <span id="source-1"></span> [Anthropic · Introducing Claude Fable 5.2 and Claude Mythos 5.2](https://www.anthropic.com/claude-fable-and-mythos-5-2)

[2] <span id="source-2"></span> [LG AI Research · K-EXAONE 2.5 API Available for Free](https://www.lgresearch.ai/news/view?seq=755)

[3] <span id="source-3"></span> [OpenAI · ChatGPT Enterprise now available in AWS Marketplace](https://openai.com/index/chatgpt-enterprise-aws-marketplace)
