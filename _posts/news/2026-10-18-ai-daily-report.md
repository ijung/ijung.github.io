---
title: "AI 데일리 리포트 — 2026-10-18"
date: "2026-10-18 06:03:28 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-10-18/
---

검수 기준: 2026-10-18 06:03 KST

## 30초 브리핑

- **DeepSeek가 Flash-1017로 모델을 갱신했습니다.** 기존 API 호출명과 모델 요금은 유지합니다.<a href="#source-1">[1]</a> 검색·페이지 수집 API도 공개했으며, 호출 경로별 과금 조건은 구분됩니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>
- **ChatGPT에서 슬라이드 공동 편집이 시작됩니다.** Space 접근권이 있는 일부 Pro·Business·Enterprise 이용자부터 순차 제공됩니다.<a href="#source-4">[4]</a>
- **GPT-6 Pro에 병렬 작업 기능이 추가됩니다.** 대상 Pro 이용자부터 배포하며, 관리형 워크스페이스는 설정과 배포 대상 여부에 따라 이용 가능 여부가 달라집니다.<a href="#source-4">[4]</a>

## 오늘의 주요 AI 뉴스

### DeepSeek Flash-1017 공개…모델 호출명 유지, 검색·수집 API 개방

**DeepSeek가 웹·앱·API 모델을 DeepSeek-V4.1-Flash-1017로 갱신하고 자체 검색 엔진의 Search·Fetch API를 공개했습니다.**<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

- **달라진 점:** API에서는 기존 `deepseek-flash` 호출명과 모델 요금을 유지합니다.<a href="#source-1">[1]</a> 회사는 작업 완료 시간과 출력 길이를 줄였다고 설명했지만, 이는 회사 자체 평가이며 이번 조사에서 독립적으로 재현하지 않았습니다.<a href="#source-1">[1]</a>
- **이용 조건:** Search와 Fetch는 각각 결과가 있는 성공 요청 1,000회당 0.75달러입니다.<a href="#source-3">[3]</a> 실패·빈 검색 결과·시간 초과에는 과금하지 않습니다.<a href="#source-3">[3]</a>
  DeepSeek API 이용자가 DeepSeek Harness·Claude Code·Codex의 서버 측 도구로 호출하는 경우는 현재 무료이며, **2026-11-01 01:00 KST부터 과금**합니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>

<p id="def-search-api">Search API는 검색어에 맞는 웹페이지를 찾아 구조화된 결과를 돌려주는 인터페이스입니다.<a href="#source-3">[3]</a></p>

<p id="def-fetch-api">Fetch API는 지정한 웹페이지 하나의 내용을 가져와 Markdown으로 돌려주는 인터페이스입니다.<a href="#source-3">[3]</a></p>

- **의미 — 해석:** 모델 응답 생성과 웹 정보 접근이 별도 서비스로 제공되므로, 모델 요금 유지와 검색·수집 비용 신설을 구분해서 볼 필요가 있습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>

<figure><img src="/assets/img/news/2026-10-18-deepseek-search-fetch.svg" alt="Search API는 검색어를 받아 검색 결과를, Fetch API는 웹페이지 주소를 받아 페이지 내용을 반환하는 비교 도식" loading="lazy" style="max-width:100%;height:auto"><figcaption>Clark 제작 설명용 도식 · AI 생성 활용. 공식 제품 화면이 아니며, 공식 문서의 입력·출력을 코드 SVG로 비교했습니다.<a href="#source-3">[3]</a></figcaption></figure>

<p class="news-source"><a href="https://api-docs.deepseek.com/updates">DeepSeek · 10월 17일 모델·검색 API 변경 기록</a><br><span class="source-published">게시: 확인 필요 (공식 변경 항목: 2026-10-17, 시각 미공개)</span></p>

<p class="news-source"><a href="https://api-docs.deepseek.com/news/news261017">DeepSeek · Flash-1017·Search·Fetch 공개</a><br><span class="source-published">게시: 확인 필요 (공식 사건일: 2026-10-17, 시각 미공개)</span></p>

<p class="news-source"><a href="https://api-docs.deepseek.com/guides/search">DeepSeek · Search·Fetch 이용·과금 안내</a><br><span class="source-published">게시: 확인 필요 (공개는 2026-10-17 공식 발표로 확인)</span></p>

### ChatGPT Slides, 생성부터 공동 편집·내보내기까지 지원

**OpenAI가 대화·파일·개요를 바탕으로 발표 자료를 만들고 팀과 함께 편집하는 ChatGPT Slides를 순차 제공합니다.**<a href="#source-4">[4]</a>

- **달라진 점:** 텍스트·이미지·도형·차트를 직접 편집하거나 ChatGPT에 수정을 요청하고, 동료와 실시간 공동 편집·댓글 작성을 할 수 있습니다.<a href="#source-4">[4]</a> 웹에서는 PowerPoint·PDF·Google Slides로 내보낼 수 있으며, 데스크톱에서는 해당 앱이 설치된 경우 PowerPoint·Keynote로 열 수 있습니다.<a href="#source-4">[4]</a>
- **대상과 의미 — 해석:** Space 접근권이 있는 대상 Pro·Business·Enterprise 이용자의 웹·데스크톱부터 제공합니다.<a href="#source-4">[4]</a> 초안 생성 뒤 별도 도구로 옮기기 전에 같은 작업 공간에서 함께 수정하고 발표할 수 있다는 변화입니다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://help.openai.com/en/articles/6825453-chatgpt-release-notes">OpenAI · ChatGPT 10월 16일 업데이트</a><br><span class="source-published">게시: 확인 필요 (업데이트 항목: 2026-10-16, 시각 미공개)</span></p>

### GPT-6 Pro, 여러 작업을 나눠 동시에 수행하는 기능 추가

**OpenAI가 ChatGPT 채팅 탭의 GPT-6 Pro에 응답 속도·도구 활용 개선과 여러 에이전트의 동시 작업 기능을 발표했습니다.**<a href="#source-4">[4]</a>

<p id="def-parallel-agents">병렬 에이전트는 하나의 작업을 여러 부분으로 나누어 여러 에이전트가 동시에 처리하는 방식입니다.<a href="#source-4">[4]</a> 이번 업데이트에서는 대화에서 진행 상황을 보고, 완료된 작업을 열어 각 에이전트가 수행한 내용을 확인할 수 있습니다.<a href="#source-4">[4]</a></p>

- **제공 상태:** 대상 Pro 이용자에게 순차 배포하며, 관리형 워크스페이스는 조직 설정과 배포 대상 여부에 따라 달라집니다.<a href="#source-4">[4]</a> 모든 계정에 즉시 제공되거나 별도 API 모델이 출시됐다는 발표는 아닙니다.<a href="#source-4">[4]</a>
- **의미 — 해석:** 답변 속도뿐 아니라 작업을 나눠 수행하는 과정과 결과를 이용자가 확인하는 방식이 함께 바뀝니다.<a href="#source-4">[4]</a> 회사가 설명한 개선이며, 속도 향상의 정량 수치나 독립 시험 결과는 이번 자료에 없습니다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://help.openai.com/en/articles/6825453-chatgpt-release-notes">OpenAI · ChatGPT 10월 16일 업데이트</a><br><span class="source-published">게시: 확인 필요 (업데이트 항목: 2026-10-16, 시각 미공개)</span></p>

## AI 트렌드와 변화 신호

- **관찰 — 생성 이후의 작업:** 오늘 확인한 변화는 모델 갱신에 그치지 않고, 웹 정보 접근·발표 자료 공동 편집·여러 작업의 동시 수행으로 이어집니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-4">[4]</a> 이는 오늘 자료에서 도출한 흐름이며 업계 전체 관심도 상승을 측정한 결과는 아닙니다.
- **전날 대비:** 전날 다룬 Claude Code Review 정식 제공·Meta 모델 가중치 공개·OpenAI 시험용 지갑 사고 보고는 반복하지 않았습니다. 오늘은 DeepSeek의 10월 17일 갱신과 ChatGPT의 10월 16일 신규 기능을 별개 사건으로 확인했습니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a>

## 조사 한계

- 최근 24시간을 우선 조사하고, ChatGPT의 10월 16일 발표는 최대 72시간 보완 범위에서 포함했습니다.<a href="#source-4">[4]</a> 공식 자료에 발표 시각이 없어 24시간 안에 해당하는지까지 확정하지 않았습니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a>
- DeepSeek 발표문·안내 문서와 ChatGPT 누적 릴리스 노트의 최초 페이지 게시 일시는 확인되지 않았습니다. DeepSeek 공식 변경 기록의 10월 17일 항목과 ChatGPT 릴리스 노트의 10월 16일 항목을 최신성 근거로 사용했습니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a>
- ChatGPT 공식 본문은 추출 경로로 확인했지만, 별도 원시 HTML 요청은 접근이 제한돼 게시 시각 메타데이터를 대조하지 못했습니다. 제공 조건과 성능 설명은 회사 발표 기준이며, 한국 계정의 실제 이용 화면까지 확인하지 않았습니다.
- 규제·정책 및 투자·파트너십에서는 이번 조사로 새 실질 변화를 확정하지 못했습니다. 해당 분야에 변화가 없다는 의미는 아닙니다.

## Sources

[1] <span id="source-1"></span> [DeepSeek · 10월 17일 모델·검색 API 변경 기록](https://api-docs.deepseek.com/updates)

[2] <span id="source-2"></span> [DeepSeek · Flash-1017·Search·Fetch 공개](https://api-docs.deepseek.com/news/news261017)

[3] <span id="source-3"></span> [DeepSeek · Search·Fetch 이용·과금 안내](https://api-docs.deepseek.com/guides/search)

[4] <span id="source-4"></span> [OpenAI · ChatGPT 10월 16일 업데이트](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)
