---
title: "AI 데일리 리포트 — 2026-10-08"
date: "2026-10-08 12:20:34 +0900"
categories: [뉴스, AI]
tags: [AI, 데일리-리포트]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-10-08/
---

갱신 기준: 2026-10-08 16:14 KST

## 30초 브리핑

- OpenAI가 ChatGPT에 버튼·폼·차트로 답하는 GPT-6·Intelligent UI의 순차 배포를 시작했다.<a href="#source-1">[1]</a>
- Anthropic이 Haiku 5.5를 공개하고 프롬프트 10만 토큰 이하·초과 요청에 서로 다른 단가를 적용했다.<a href="#source-2">[2]</a>
- LlamaIndex가 여러 문서 처리 모델을 하나의 API로 쓰는 OpenDocRouter를 공개했다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### 1. OpenAI, ChatGPT 답변에 직접 조작하는 화면 도입

<p><strong><span id="def-intelligent-ui">Intelligent UI는 ChatGPT 답변 안에 버튼·폼·차트 같은 조작 가능한 요소를 넣는 이번 발표의 기능명이다.</span></strong><a href="#source-1">[1]</a></p>

- GPT-6로 질문에 맞는 도구를 대화 안에서 사용하는 변화다.<a href="#source-1">[1]</a> 이번 변경은 **Chat 탭에 한정되며 Work·Codex 모델은 바뀌지 않는다.**<a href="#source-1">[1]</a>
- 발표일인 10월 7일 Plus·Pro·Business·Enterprise부터 글로벌 순차 배포를 시작하고, 다음 날 Free·Go로 확대한다고 밝혔다.<a href="#source-1">[1]</a> Enterprise는 관리자 설정에 따른다.<a href="#source-1">[1]</a>

<p><span id="def-system-card">시스템 카드는 모델의 평가 방법·결과와 보호 조치를 설명하는 공개 문서다.</span><a href="#source-4">[4]</a> 이 문서는 일부 안전성 평가에서 이전 모델보다 낮은 결과도 보고했다.<a href="#source-4">[4]</a> <span id="def-system-safeguards">시스템 수준 보호 조치는 모델 자체의 응답 외에 서비스가 적용하는 보호 장치다.</span><a href="#source-4">[4]</a> 이를 제외한 모델 평가이므로 서비스 전체 안전성의 개선·악화로 일반화할 수 없다.<a href="#source-4">[4]</a></p>

<p class="news-source"><a href="https://openai.com/index/gpt-6-for-everyone">OpenAI · GPT-6·Intelligent UI</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-1">[1]</a></p>

<p class="news-source"><a href="https://cdn.openai.com/pdf/gpt-6-october.pdf">OpenAI · GPT-6 10월 시스템 카드</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-4">[4]</a></p>

### 2. Anthropic, Haiku 5.5 출시…요청 길이별로 다른 단가

**대량·비용 민감 작업을 겨냥한 소형 모델 Haiku 5.5가 공개됐고, Copilot에도 점진 배포된다.**<a href="#source-2">[2]</a><a href="#source-5">[5]</a>

<p><span id="def-prompt">프롬프트는 모델에 보내는 요청 내용이다.</span><a href="#source-2">[2]</a><a href="#source-6">[6]</a> 아래 단가는 <span id="def-input">입력, 즉 모델이 받는 내용</span>과 <span id="def-output">출력, 즉 모델이 생성하는 답변</span>에 각각 적용된다.<a href="#source-6">[6]</a><a href="#source-2">[2]</a></p>

- **100만 토큰당 일반 입력/출력 가격:** 프롬프트 10만 토큰 이하는 **0.10/0.50달러**, 초과 요청은 **0.50/2.50달러**다.<a href="#source-2">[2]</a> 프롬프트 캐시 읽기·쓰기 가격과는 별개다.<a href="#source-2">[2]</a>
- Anthropic의 “Haiku 4.5보다 평균 실행 비용 약 75% 감소”는 짧고 긴 요청의 비중과 새 토크나이저를 반영한 자체 계산이다.<a href="#source-2">[2]</a> 모든 요청이 같은 비율로 저렴해진다는 뜻은 아니다.<a href="#source-2">[2]</a>
- Claude Platform·AWS·Google Cloud·Azure에서 제공한다고 밝혔다.<a href="#source-2">[2]</a> Copilot Pro·Pro+·Max·Business·Enterprise에서는 일반 제공을 시작하되 계정별로 순차 적용하며, 조직의 모델 접근은 관리자 설정에 따른다.<a href="#source-5">[5]</a>

<figure><img src="/assets/img/news/2026-10-08-haiku-5-5-price-conditions.svg" alt="Haiku 5.5 프롬프트 10만 토큰 이하와 초과 요청의 일반 입력·출력 단가 비교. 단위: 미국 달러/100만 토큰." loading="lazy" width="440" height="474" style="max-width:100%;height:auto"><figcaption>Clark 제작 설명용 도식 · AI 생성 활용<br>공식 제품 화면이 아닌 설명용 도식입니다.<br>코드 기반 SVG · 근거: <a href="https://www.anthropic.com/claude-haiku-5-5">Anthropic 공식 가격표</a><a href="#source-2">[2]</a></figcaption></figure>

<p class="news-source"><a href="https://www.anthropic.com/claude-haiku-5-5">Anthropic · Haiku 5.5와 요금 변경</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-2">[2]</a></p>

<p class="news-source"><a href="https://github.blog/changelog/2026-10-07-claude-haiku-5-5-in-github-copilot">GitHub · Copilot의 Haiku 5.5</a><br><span class="source-published">게시: 2026-10-08 05:12 KST</span><br><span class="source-modified">수정: 2026-10-08 05:39 KST</span><a href="#source-5">[5]</a></p>

### 3. LlamaIndex, 문서 처리 모델을 바꿔 쓰는 OpenDocRouter 공개

**PDF·PNG·JPEG를 텍스트 문서(Markdown)로 변환하는 여러 모델을 하나의 API로 제공한다.**<a href="#source-3">[3]</a>

- <span id="def-recipe">문서 처리 레시피는 OpenDocRouter가 모델별로 묶어 둔 프롬프트·처리 과정·설정이다.</span><a href="#source-3">[3]</a><a href="#source-7">[7]</a> 결과가 달라질 수 있는 변경은 버전으로 구분한다.<a href="#source-7">[7]</a>
- <span id="def-layout">문서 레이아웃은 페이지 안의 요소 위치와 읽기 순서 정보다.</span><a href="#source-7">[7]</a> `layout` 옵션을 켜면 <span id="def-box">경계 상자, 즉 각 요소가 원문에서 차지하는 사각 영역</span>도 받는다.<a href="#source-3">[3]</a><a href="#source-7">[7]</a> 가입 후 API 키를 만들어 사용할 수 있다고 발표했다.<a href="#source-3">[3]</a>

**최소 충전액은 공식 자료끼리 다르다:** 출시 글은 **25달러부터**, 현재 Docs는 **10달러부터·충전액의 5% 수수료**라고 적고 있다.<a href="#source-3">[3]</a><a href="#source-7">[7]</a> 변경 시점과 실제 결제 화면을 확인하지 못해 하나의 확정값으로 제시하지 않는다.

<p class="news-source"><a href="https://www.llamaindex.ai/blog/introducing-opendocrouter">LlamaIndex · OpenDocRouter 출시</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-3">[3]</a></p>

<p class="news-source"><a href="https://www.opendocrouter.ai/docs">OpenDocRouter · 이용·과금 Docs</a><br><span class="source-published">게시: 확인 필요 · 현행 이용 문서</span><a href="#source-7">[7]</a></p>

## 기타 뉴스

- **NVIDIA·Microsoft, Windows에서 에이전트를 실행하는 기반 공개** — NVIDIA 발표 기준으로 MXC는 일반 제공, RTX Spark 노트북은 예약 주문, DGX Station for Windows는 정식 제공 전 미리보기다.<a href="#source-8">[8]</a> <span id="def-mxc">Microsoft Execution Containers(MXC)는 에이전트를 운영체제 통제 아래 백그라운드에서 지속 실행하도록 하는 기반 기능명이다.</span><a href="#source-8">[8]</a> 여기서 <span id="def-agent">AI 에이전트는 모델이 작업 진행과 도구 사용을 결정하며 일을 수행하는 시스템이다.</span><a href="#source-9">[9]</a>

  <p class="news-source"><a href="https://blogs.nvidia.com/blog/local-ai-rtx-spark-microsoft-windows-event">NVIDIA · Windows 로컬 에이전트 발표</a><br><span class="source-published">게시: 2026-10-08 03:45 KST</span><br><span class="source-modified">수정: 2026-10-08 06:40 KST</span><a href="#source-8">[8]</a></p>

## AI 트렌드와 변화 신호

- **해석 — 단가와 실제 비용을 나눠 봐야 한다.** Haiku의 요청 길이별 단가와 공급자 평균 절감률은 다른 지표이며, OpenDocRouter는 출시 글과 현행 문서의 최소 충전액도 다르다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a><a href="#source-7">[7]</a>
- **관찰 — 모델 외의 사용 환경도 바뀌었다.** 대화 속 인터페이스, 개발 도구의 모델 선택, 운영체제의 에이전트 실행 기반이 각각 구체적인 제품 변경으로 이어졌다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a><a href="#source-8">[8]</a>

## 오늘의 용어

- **토큰:** 모델이 글을 처리하는 단위로, 단어 전체나 일부·문자 등에 대응한다.<a href="#source-6">[6]</a> 오늘 가격의 “100만 토큰당”은 글자 수나 요청 횟수와 다르다.<a href="#source-6">[6]</a>
- **토크나이저:** 글을 모델이 처리하는 토큰 단위로 나누는 도구다.<a href="#source-6">[6]</a> Haiku 5.5의 새 토크나이저는 작업당 토큰 수에도 영향을 주므로 단가 인하율과 평균 비용 절감률이 같지 않다.<a href="#source-2">[2]</a>
- **프롬프트 캐시:** 반복되는 요청 앞부분을 저장해 재사용하는 기능이다.<a href="#source-10">[10]</a> 쓰기는 저장, 읽기는 재사용에 해당하며 일반 입력과 별도 단가가 적용된다.<a href="#source-10">[10]</a><a href="#source-2">[2]</a>

## 조사 한계

- 최근 24시간의 실질 변화를 우선하고 필요한 배경은 최대 72시간까지 확인했다. 날짜만 공개된 발표는 24시간 경계 포함 여부를 확정하지 않았다. 저장소에 전날 AI 전용 보고서가 없어 **전날 비교 기준 없음**.
- OpenAI 본문은 원문 추출로 읽었으나 직접 HTML 접근이 제한돼 게시 시각을 확인하지 못했다. OpenDocRouter Docs는 최초 게시·갱신 일시 미공개인 현행 이용 문서로 참고했다.
- 제품·가격·제공 상태는 기업 발표와 공식 문서 기준이다. 기업 발표는 독립 검증이 아니며 실제 작업별 절감률·국내 계정별 배포 완료·결제 조건은 시험하지 않았다. NVIDIA 소식은 파트너 발표로 확인했으며 Microsoft 별도 원자료와 실물 공급은 검증하지 않았다.
- 공식 미디어는 재게시 권리가 확인되지 않아 복제하지 않았다. 도식은 공식 가격표에 근거한 설명용이며 실제 브라우저 화면 검수는 수행하지 않았다.

## Sources

[1] <span id="source-1"></span> [OpenAI · GPT-6·Intelligent UI](https://openai.com/index/gpt-6-for-everyone)

[2] <span id="source-2"></span> [Anthropic · Haiku 5.5와 요금 변경](https://www.anthropic.com/claude-haiku-5-5)

[3] <span id="source-3"></span> [LlamaIndex · OpenDocRouter 출시](https://www.llamaindex.ai/blog/introducing-opendocrouter)

[4] <span id="source-4"></span> [OpenAI · GPT-6 10월 시스템 카드](https://cdn.openai.com/pdf/gpt-6-october.pdf)

[5] <span id="source-5"></span> [GitHub · Copilot의 Haiku 5.5](https://github.blog/changelog/2026-10-07-claude-haiku-5-5-in-github-copilot)

[6] <span id="source-6"></span> [Claude Docs · 토큰 용어집](https://platform.claude.com/docs/en/about-claude/glossary)

[7] <span id="source-7"></span> [OpenDocRouter · 이용·과금 Docs](https://www.opendocrouter.ai/docs)

[8] <span id="source-8"></span> [NVIDIA · Windows 로컬 에이전트 발표](https://blogs.nvidia.com/blog/local-ai-rtx-spark-microsoft-windows-event)

[9] <span id="source-9"></span> [Anthropic · AI 에이전트 설명](https://www.anthropic.com/engineering/building-effective-agents)

[10] <span id="source-10"></span> [Claude Docs · 프롬프트 캐시](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
