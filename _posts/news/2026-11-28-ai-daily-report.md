---
title: "AI 데일리 리포트 — 2026-11-28"
date: "2026-11-28 06:00:33 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-28/
---

검수 기준: 2026-11-28 06:10 KST

## 30초 브리핑

- OpenAI가 GPT-6.2 사용량 계산 오류를 수정했습니다.<a href="#source-1">[1]</a> 같은 작업을 해도 지난주보다 이용 한도를 더 빨리 소진한다는 설명입니다.<a href="#source-1">[1]</a>
- Claude Duet의 인도 사전 공개를 확인했습니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>
  개인 유료 요금제는 대기명단 방식이며, 조직용 제공 조건은 보도와 공식 안내가 엇갈립니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>
- OpenAI가 일부 기업 고객과 Private Inference 시험을 시작했다는 보도가 나왔습니다.<a href="#source-4">[4]</a> 일반 제공이 아니라 초기 시험 단계입니다.<a href="#source-5">[5]</a>

## 오늘의 주요 AI 뉴스

### GPT-6.2, 요금 인상이 아니라 사용량 계산 오류 수정

**OpenAI 관계자는 GPT-6.2 Astra·Sol 사용량에 잘못 적용되던 할인을 수정했다고 설명했습니다.**<a href="#source-1">[1]</a>

- **달라진 점:** The Next Web은 같은 작업 기준으로 지난주보다 Astra의 한도 도달이 40%, Sol은 25% 빨라진다는 설명을 전했습니다.<a href="#source-1">[1]</a> 이는 모델 성능이나 API 토큰 단가의 변화율이 아닙니다.
- **왜 중요한지:** 전날 다룬 Chat·Work 한도 통합과는 별개의 계산 오류 수정입니다.<a href="#source-1">[1]</a> 과거 사용분의 차액은 소급 청구하지 않으며, Go·Plus·Pro·Business에는 나중에 쓸 수 있는 한도 초기화 기회 한 번을 추가한다는 설명입니다.<a href="#source-1">[1]</a>

수치는 회사 관계자의 설명을 인용한 보도 기준이며 실제 계정별 사용량을 측정한 결과는 아닙니다. 11월 26일 발표를 이번 회차에 뒤늦게 확인했습니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://thenextweb.com/news/openai-billing-mistake-gpt-6-2-usage-limits">The Next Web · OpenAI fixes a GPT-6.2 billing mistake, and users hit limits 40% faster</a><br><span class="source-published">게시: 2026-11-26 23:33 KST</span></p>

### Claude Duet, 인도로 확대…조직용 제공 조건은 확인 필요

**FoneArena는 Anthropic의 업무형 AI 에이전트 Claude Duet 사전 공개가 인도로 확대됐다고 보도했으며, 현재 공식 제품 페이지에도 인도가 포함돼 있습니다.**<a href="#source-2">[2]</a><a href="#source-3">[3]</a>

- **달라진 점:** 공식 안내는 Pro·Max·Ultra 이용자의 대기명단 참여를 명시합니다.<a href="#source-3">[3]</a> <span id="def-preview">리서치 프리뷰는 기능과 이용 조건을 시험하는 연구용 사전 공개 단계입니다.</span><a href="#source-3">[3]</a>
- **왜 중요한지:** <span id="def-agent">AI 에이전트는 연결된 앱과 도구를 사용해 이용자가 맡긴 업무를 수행하는 AI입니다.</span><a href="#source-3">[3]</a> Claude Duet는 클라우드의 자체 컴퓨터에서 작업을 이어가며, 메시지 전송·결제 같은 민감한 작업 전에 승인을 구하도록 설계됐습니다.<a href="#source-3">[3]</a> 실제 안전성을 독립 검증한 결론은 아닙니다.

**조건 상충:** 보도는 Team 공개 베타·Enterprise 비공개 베타를 안내하지만, 이번에 읽은 공식 FAQ는 두 요금제가 아직 포함되지 않았다고 명시합니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a> 조직용 제공을 확정하지 않고 현재 공식 안내를 우선합니다. 백그라운드 확인 작업도 기존 Claude 요금제 한도를 사용합니다.<a href="#source-3">[3]</a>

11월 26일 보도를 이번 회차에 뒤늦게 확인했습니다. 한국 제공 여부는 이번 조사에서 확인하지 못했습니다.

<p class="news-source"><a href="https://www.fonearena.com/blog/497616/anthropic-claude-duet-india.html">FoneArena · Anthropic expands Claude Duet AI agent research preview to India</a><br><span class="source-published">게시: 2026-11-26 15:23 KST</span></p>
<p class="news-source"><a href="https://claude.com/product/claude-duet">Anthropic · Claude Duet</a><br><span class="source-published">게시: 확인 필요 · 본문 사건일: 2026-11-26</span></p>

### OpenAI Private Inference, 일부 기업 고객 대상으로 시험

**The Next Web은 OpenAI가 소수 기업 고객과 Private Inference 시험을 시작했다고 보도했으며, 공식 페이지도 이를 Alpha로 표시합니다.**<a href="#source-4">[4]</a><a href="#source-5">[5]</a>

- **달라진 점:** <span id="def-private-inference">Private Inference는 모델이 처리하는 동안 입력과 응답을 보호하도록 기밀 컴퓨팅과 엄격한 통제를 결합한 OpenAI 기능입니다.</span><a href="#source-5">[5]</a>
- **왜 중요한지:** <span id="def-confidential">기밀 컴퓨팅은 처리 중인 데이터를 격리된 하드웨어 환경에서 보호하는 방식입니다.</span><a href="#source-4">[4]</a> 저장 여부뿐 아니라 AI가 데이터를 처리하는 동안의 보호를 다룬다는 의미가 있습니다.<a href="#source-5">[5]</a>

공식 제공 설명과 시험 보도를 확인한 것이며, 보호 효과를 직접 시험한 결과는 아닙니다. 광범위한 제공 일정은 공개되지 않았다는 보도입니다.<a href="#source-4">[4]</a> 11월 25일 시험 발표를 이번 회차에 뒤늦게 확인했습니다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://thenextweb.com/news/openai-private-inference-testing-customers">The Next Web · OpenAI is testing a way to shield business prompts while its models run</a><br><span class="source-published">게시: 2026-11-26 04:27 KST</span></p>
<p class="news-source"><a href="https://openai.com/business/private-intelligence">OpenAI · Private Intelligence</a><br><span class="source-published">게시: 확인 필요 · 본문 사건일: 2026-11-25</span></p>

## 기타 뉴스

<ul>
<li><strong>캐나다, AI 연산자원 지원기금에 7억 캐나다달러 추가</strong><br>캐나다 정부는 AI Compute Access Fund를 기존 3억에서 총 10억 캐나다달러로 확대하고, 보건·생명과학 분야의 신규 신청을 열었다고 발표했습니다.<a href="#source-6">[6]</a> 캐나다에서 소유·운영·관리되는 연산자원 이용을 장려하는 정책입니다.<a href="#source-6">[6]</a> 11월 26일 공식 발표를 이번 회차에 뒤늦게 확인했습니다.
<p class="news-source"><a href="https://www.canada.ca/en/innovation-science-economic-development/news/2026/11/government-of-canada-boosts-the-ai-compute-access-fund-with-700-million-expansion.html">캐나다 정부 · Government of Canada boosts AI innovation from Canadian SMEs with $700 million expansion of the AI Compute Access Fund</a><br><span class="source-published">게시: 2026-11-26 · 시각 미공개</span></p>
</li>
<li><strong>영국 금융감독당국, Meta Muse 공동 시험 제안 거절 설명</strong><br>WIRED는 FCA 관계자가 Meta의 개인 비서형 AI Muse에 대한 공동 시험 제안이 거절됐다고 말했다고 보도했습니다.<a href="#source-7">[7]</a> Meta는 관련 규제당국과 건설적으로 협의해 왔다고 답변했습니다.<a href="#source-7">[7]</a> 공동 시험 여부에 관한 취재 보도이며, 위법 판단이나 판매 금지 결정은 아닙니다. 11월 26일 보도를 이번 회차에 뒤늦게 확인했습니다.
<p class="news-source"><a href="https://www.wired.com/story/meta-refused-to-share-muse-for-testing-uk-financial-watchdog-complains">WIRED · Meta Refused to Share Muse for Testing, UK Financial Watchdog Complains</a><br><span class="source-published">게시: 2026-11-26 19:51 KST</span></p>
</li>
</ul>

## AI 트렌드와 변화 신호

- **실행 기능만큼 중요한 한도·승인 조건입니다:** GPT-6.2 계산 수정과 Claude Duet의 백그라운드 사용량 차감은 같은 구독에서도 작업 방식에 따라 한도 계획을 다시 살펴봐야 하는 사례입니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>
  Duet의 민감 작업 승인 설계와 Muse 공동 시험 논의는 에이전트의 실행 경계를 확인해야 한다는 의미가 있습니다.<a href="#source-3">[3]</a><a href="#source-7">[7]</a>
- **기업 AI의 데이터·연산 통제를 관찰합니다:** Private Inference의 처리 중 보호와 캐나다의 자국 연산자원 장려는 각각 데이터 보호 방식과 인프라 통제를 구체화한 사례입니다.<a href="#source-5">[5]</a><a href="#source-6">[6]</a> 시장 전체의 관심도 상승을 측정한 결론은 아닙니다.
- **전날 대비:** 전날 소개한 Chat·Work 한도 통합, ‘모두의 AI’, 뉴욕주 장난감 규제 등과 사건을 대조해 반복 보도를 제외했습니다. 오늘은 사용량 계산 수정과 사전 공개 지역·조건을 추가 확인했으며, 지난 발표를 오늘의 새 출시로 바꾸지 않았습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a>

## 조사 한계

- 수집 기준은 2026-11-28 06:03:33 KST입니다. 최근 24시간을 우선 확인했으며, 이번에 채택한 소식은 최대 72시간 안의 미보도 발표를 뒤늦게 확인한 누락 보완입니다. 새 모델 출시나 전면 제공으로 확대 해석하지 않았습니다.
- OpenAI·Anthropic·Google DeepMind·GitHub의 공식 목록을 직접 읽었지만, 첫 화면 범위와 월 단위 날짜·상단 날짜 불일치 때문에 전체 기간의 누락 여부는 확정하지 못했습니다. 다른 보안·투자 후보는 원자료·시점 검증 범위가 부족해 제외했습니다.
- 날짜만 공개된 정부 발표와 게시일 미공개 제품 페이지는 시각을 추정하지 않았습니다. 캐나다 원문은 웹 추출로 읽었으나 별도 직접 접속은 시간 초과로 재확인하지 못했습니다. 기업 기능·보호 효과는 발표 기준이며 실제 제품 시험 결과가 아닙니다. 필요한 용어는 관련 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://thenextweb.com/news/openai-billing-mistake-gpt-6-2-usage-limits">The Next Web · OpenAI fixes a GPT-6.2 billing mistake, and users hit limits 40% faster</a></p>

<p id="source-2" markdown="0">[2] <a href="https://www.fonearena.com/blog/497616/anthropic-claude-duet-india.html">FoneArena · Anthropic expands Claude Duet AI agent research preview to India</a></p>

<p id="source-3" markdown="0">[3] <a href="https://claude.com/product/claude-duet">Anthropic · Claude Duet</a></p>

<p id="source-4" markdown="0">[4] <a href="https://thenextweb.com/news/openai-private-inference-testing-customers">The Next Web · OpenAI is testing a way to shield business prompts while its models run</a></p>

<p id="source-5" markdown="0">[5] <a href="https://openai.com/business/private-intelligence">OpenAI · Private Intelligence</a></p>

<p id="source-6" markdown="0">[6] <a href="https://www.canada.ca/en/innovation-science-economic-development/news/2026/11/government-of-canada-boosts-the-ai-compute-access-fund-with-700-million-expansion.html">캐나다 정부 · Government of Canada boosts AI innovation from Canadian SMEs with $700 million expansion of the AI Compute Access Fund</a></p>

<p id="source-7" markdown="0">[7] <a href="https://www.wired.com/story/meta-refused-to-share-muse-for-testing-uk-financial-watchdog-complains">WIRED · Meta Refused to Share Muse for Testing, UK Financial Watchdog Complains</a></p>
