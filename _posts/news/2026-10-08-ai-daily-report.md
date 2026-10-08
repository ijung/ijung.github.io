---
title: "AI 데일리 리포트 — 2026-10-08"
date: "2026-10-08 12:20:34 +0900"
categories: [뉴스, AI]
tags: [AI, 데일리-리포트]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-10-08/
---

갱신 기준: 2026-10-08 14:56 KST

## 30초 브리핑

- OpenAI가 ChatGPT에 버튼·폼·차트로 답하는 GPT-6·Intelligent UI의 순차 배포를 시작했다.<a href="#source-1">[1]</a>
- Anthropic이 Haiku 5.5를 공개하고 프롬프트 10만 토큰 이하·초과에 서로 다른 낮은 단가를 적용했다.<a href="#source-2">[2]</a>
- LlamaIndex가 여러 문서 처리 모델을 하나의 API로 쓰는 OpenDocRouter를 공개했다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### 1. OpenAI, ChatGPT 답변에 직접 조작하는 UI 도입

**GPT-6·Intelligent UI로 대화 안에서 버튼을 누르고 폼·차트를 조작할 수 있게 된다.**<a href="#source-1">[1]</a>

- 텍스트만 읽는 대신 질문에 맞는 도구를 답변 안에서 사용하는 변화다.<a href="#source-1">[1]</a> 이번 변경은 **Chat 탭에 한정되며 Work·Codex 모델은 바뀌지 않는다.**<a href="#source-1">[1]</a>
- 발표일인 10월 7일 Plus·Pro·Business·Enterprise부터 글로벌 순차 배포를 시작하고, 다음 날 Free·Go로 확대한다고 밝혔다.<a href="#source-1">[1]</a> Enterprise는 관리자 설정에 따른다.<a href="#source-1">[1]</a>

시스템 카드는 일부 모델 안전성 평가에서 통계적으로 유의한 후퇴도 보고했다.<a href="#source-4">[4]</a> 시스템 수준 보호 조치를 제외한 모델 평가이므로, 서비스 전체가 더 안전해졌거나 덜 안전해졌다는 결론으로 일반화할 수 없다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://openai.com/index/gpt-6-for-everyone">OpenAI · GPT-6·Intelligent UI</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-1">[1]</a></p>

<p class="news-source"><a href="https://cdn.openai.com/pdf/gpt-6-october.pdf">OpenAI · GPT-6 10월 시스템 카드</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-4">[4]</a></p>

### 2. Anthropic, Haiku 5.5 출시…짧은 요청과 긴 요청의 단가 분리

**Haiku 5.5의 비용은 요청 크기에 따라 달라지고, Copilot에도 점진 배포된다.**<a href="#source-2">[2]</a><a href="#source-5">[5]</a>

- **100만 토큰당 일반 입력/출력 가격:** 프롬프트 10만 토큰 이하는 **0.10/0.50달러**, 초과 요청은 **0.50/2.50달러**다.<a href="#source-2">[2]</a> 캐시 읽기·쓰기 가격과는 별개다.<a href="#source-2">[2]</a>
- Anthropic의 “Haiku 4.5보다 평균 실행 비용 약 75% 감소”는 요청 분포와 새 토크나이저를 반영한 자체 계산이다.<a href="#source-2">[2]</a> 모든 요청이 같은 비율로 저렴해진다는 뜻은 아니다.<a href="#source-2">[2]</a>
- Claude Platform·AWS·Google Cloud·Azure에서 제공한다고 밝혔다.<a href="#source-2">[2]</a> Copilot Pro·Pro+·Max·Business·Enterprise에서는 일반 제공을 시작하되 점진 배포하며, 조직 접근은 관리자 모델 정책에 따른다.<a href="#source-5">[5]</a>

<figure><img src="/assets/img/news/2026-10-08-haiku-5-5-price-conditions.svg" alt="Haiku 5.5 요청 프롬프트 10만 토큰 이하와 초과의 일반 입력·출력 단가 비교. 단위: USD/100만 토큰." loading="lazy" width="440" height="474" style="max-width:100%;height:auto"><figcaption>Clark 제작 설명용 도식 · AI 생성 활용<br>공식 제품 화면이 아니며 확인된 기사 내용을 이해하기 쉽게 표현했습니다.<br>코드 기반 SVG · 근거: <a href="https://www.anthropic.com/claude-haiku-5-5">Anthropic 공식 가격표</a><a href="#source-2">[2]</a></figcaption></figure>

<p class="news-source"><a href="https://www.anthropic.com/claude-haiku-5-5">Anthropic · Haiku 5.5와 요금 변경</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-2">[2]</a></p>

<p class="news-source"><a href="https://github.blog/changelog/2026-10-07-claude-haiku-5-5-in-github-copilot">GitHub · Copilot의 Haiku 5.5</a><br><span class="source-published">게시: 2026-10-08 05:12 KST</span><br><span class="source-modified">수정: 2026-10-08 05:39 KST</span><a href="#source-5">[5]</a></p>

### 3. LlamaIndex, 문서 처리 모델을 바꿔 쓰는 OpenDocRouter 공개

**PDF·PNG·JPEG를 Markdown으로 변환하는 여러 모델을 하나의 API로 제공한다.**<a href="#source-3">[3]</a>

- 모델마다 프롬프트·처리·설정을 묶은 버전별 레시피를 사용한다.<a href="#source-3">[3]</a> `layout` 옵션을 켜면 원문 위치를 나타내는 경계 상자와 읽기 순서 정보도 받는다.<a href="#source-3">[3]</a>
- 가입 후 API 키를 생성해 사용할 수 있다고 발표했다.<a href="#source-3">[3]</a> 모델을 바꿀 때 연결·설정을 다시 맞추는 부담을 줄이려는 접근이다.<a href="#source-3">[3]</a>

**최소 충전액은 공식 자료끼리 다르다:** 출시 글은 **25달러부터**, 현재 Docs는 **10달러부터·충전액의 5% 수수료**라고 적고 있다.<a href="#source-3">[3]</a><a href="#source-6">[6]</a> 조건 변경 시점과 실제 결제 화면을 확인하지 못해 하나의 확정값으로 제시하지 않는다.

<p class="news-source"><a href="https://www.llamaindex.ai/blog/introducing-opendocrouter">LlamaIndex · OpenDocRouter 출시</a><br><span class="source-published">게시: 2026-10-07 · 시각 미공개</span><a href="#source-3">[3]</a></p>

<p class="news-source"><a href="https://www.opendocrouter.ai/docs">OpenDocRouter · 이용·과금 Docs</a><br><span class="source-published">게시: 확인 필요 · 현행 이용 문서</span><a href="#source-6">[6]</a></p>

## 기타 뉴스

- **NVIDIA·Microsoft, Windows 로컬 에이전트 실행 기반 공개** — NVIDIA 발표 기준으로 Microsoft Execution Containers(MXC)는 일반 제공, RTX Spark 노트북은 예약 주문, DGX Station for Windows는 미리보기다.<a href="#source-7">[7]</a>

  <p class="news-source"><a href="https://blogs.nvidia.com/blog/local-ai-rtx-spark-microsoft-windows-event">NVIDIA · Windows 로컬 에이전트 발표</a><br><span class="source-published">게시: 2026-10-08 03:45 KST</span><br><span class="source-modified">수정: 2026-10-08 06:40 KST</span><a href="#source-7">[7]</a></p>

## AI 트렌드와 변화 신호

- **해석 — 단가와 실제 비용을 분리해서 볼 필요가 커졌다.** Haiku의 요청 길이별 단가와 공급자 평균 절감률은 다른 지표이고, OpenDocRouter의 최소 충전액은 현행 문서와 출시 글도 다르다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a><a href="#source-6">[6]</a>
- **관찰 — 모델 외의 사용 환경도 구체화됐다.** 대화 속 인터페이스, 개발 도구의 모델 선택, 운영체제의 에이전트 실행 기반이 각각 제품 변경으로 이어졌다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a><a href="#source-7">[7]</a>

## 조사 한계

- 최근 24시간을 우선하고 배경은 최대 72시간까지 확인했다. 날짜만 공개된 발표는 24시간 경계 포함 여부를 확정하지 않았다. 전날 AI 전용 보고서가 없어 **전날 비교 기준 없음**.
- OpenAI 제품 본문은 확보했으나 직접 HTML 접근이 제한돼 게시 시각을 확인하지 못했다. OpenDocRouter Docs는 최초 게시·갱신 일시가 미공개인 현행 이용 문서로 참고했다.
- 제품·비용·제공 상태는 기업 발표와 공식 문서 기준이다. 성능·실제 작업별 절감률, 국내 계정별 배포 완료, 실제 결제 조건은 시험하지 않았다. NVIDIA 소식은 파트너 발표로 확인했으며 Microsoft 별도 원자료와 실물 공급은 검증하지 않았다.
- 공식 미디어는 재게시 권리가 확인되지 않아 복제하지 않았다. 설명용 도식은 공식 가격표에 근거했으며, 실제 브라우저 화면 검수는 수행하지 않았다.

## Sources

[1] <span id="source-1"></span> [OpenAI · GPT-6·Intelligent UI](https://openai.com/index/gpt-6-for-everyone)

[2] <span id="source-2"></span> [Anthropic · Haiku 5.5와 요금 변경](https://www.anthropic.com/claude-haiku-5-5)

[3] <span id="source-3"></span> [LlamaIndex · OpenDocRouter 출시](https://www.llamaindex.ai/blog/introducing-opendocrouter)

[4] <span id="source-4"></span> [OpenAI · GPT-6 10월 시스템 카드](https://cdn.openai.com/pdf/gpt-6-october.pdf)

[5] <span id="source-5"></span> [GitHub · Copilot의 Haiku 5.5](https://github.blog/changelog/2026-10-07-claude-haiku-5-5-in-github-copilot)

[6] <span id="source-6"></span> [OpenDocRouter · 이용·과금 Docs](https://www.opendocrouter.ai/docs)

[7] <span id="source-7"></span> [NVIDIA · Windows 로컬 에이전트 발표](https://blogs.nvidia.com/blog/local-ai-rtx-spark-microsoft-windows-event)
