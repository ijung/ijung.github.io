---
title: "AI 데일리 리포트 — 2026-10-20"
date: "2026-10-20 09:12:46 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-10-20/
---

검수 기준: 2026-10-20 09:13 KST

## 30초 브리핑

- **GitHub가 Copilot의 기존 6개 모델 지원 종료를 공지했습니다.**<a href="#source-1">[1]</a> 대부분의 Copilot 이용 환경이 대상이며, 대체 모델은 기업 정책에 따라 별도 활성화가 필요할 수 있습니다.<a href="#source-1">[1]</a>
- **기업의 Copilot 모델 정책을 프로그램으로 조회·변경할 수 있게 됐습니다.**<a href="#source-2">[2]</a> 기업·기업 팀·기업 앱·조직의 네 수준을 지원합니다.<a href="#source-2">[2]</a>

## 오늘의 주요 AI 뉴스

### Copilot, 기존 6개 모델 지원 종료…대체 모델·기업 설정 확인 필요

**GitHub가 10월 19일을 기준으로 Gemini 3.7 Flash, GPT-5.5, GPT-5.4, GPT-5.4 mini, GPT-5 mini, Grok 4.5의 지원을 종료했다고 발표했습니다.**<a href="#source-1">[1]</a>

- **달라진 점:** 9월에 예고한 종료의 시행 공지입니다.<a href="#source-1">[1]</a> Copilot Chat, 코드 일부 수정, 질문·에이전트 모드, 코드 자동완성 등 대부분의 이용 환경이 대상입니다.<a href="#source-1">[1]</a>
- **전환 조건:** 공식 권장 대안은 아래와 같습니다.<a href="#source-1">[1]</a> Copilot Enterprise 관리자는 대체 모델의 사용을 허용해야 할 수 있으며, 기존 모델을 제거하는 별도 조치는 필요하지 않습니다.<a href="#source-1">[1]</a>

<figure><img src="/assets/img/news/2026-10-20-copilot-model-transitions.svg" alt="GitHub 공식 권장 대안: Gemini 3.7 Flash는 Gemini 3.8 Flash, GPT-5.5와 GPT-5.4는 GPT-6.1 Sol, GPT-5.4 mini와 GPT-5 mini는 GPT-6 Luna, Grok 4.5는 Grok 4.7" loading="lazy" style="max-width:100%;height:auto"><figcaption>Clark 제작 설명용 도식 · AI 생성 활용. 공식 제품 화면이 아니며, GitHub의 권장 대안 표를 코드 SVG로 정리했습니다.<a href="#source-1">[1]</a> 자동 전환을 뜻하지 않으며, 기업 정책에 따라 대체 모델 활성화가 필요할 수 있습니다.<a href="#source-1">[1]</a></figcaption></figure>

- **의미 — 해석:** 새 모델의 출시보다, 기존 개발 환경에서 사용할 수 있는 모델 목록이 바뀌었다는 점이 핵심입니다.<a href="#source-1">[1]</a> 공식 공지는 모든 이용 환경의 동시 종료나 자동 전환을 보장하지 않으므로, 이를 전체 서비스의 일괄 교체로 해석하지 않았습니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://github.blog/changelog/2026-10-19-github-copilot-models-deprecated-on-october-19-2026">GitHub · Copilot 6개 모델 지원 종료</a><br><span class="source-published">게시: 2026-10-20 06:27 KST</span><br><span class="source-modified">수정: 2026-10-20 06:59 KST</span></p>

### Copilot 모델 정책 API 정식 제공…기업·팀·앱·조직 설정 지원

**GitHub가 Copilot 모델 정책을 조회·변경하는 API를 기업·기업 팀·기업 앱·조직의 네 수준에서 정식 제공한다고 발표했습니다.**<a href="#source-2">[2]</a>

<p id="def-model-policy">모델 정책은 구성원이 Copilot에서 어떤 모델을 사용할 수 있는지 관리하는 설정입니다.<a href="#source-2">[2]</a></p>

<p id="def-api">이번 API는 관리 프로그램에서 모델 정책을 조회하거나 변경할 수 있도록 제공하는 인터페이스입니다.<a href="#source-2">[2]</a></p>

- **달라진 점:** 설정 화면을 직접 조작하는 것 외에, 프로그램을 통해 네 수준의 모델 정책을 읽거나 바꿀 수 있는 경로가 제공됐습니다.<a href="#source-2">[2]</a>
- **관리 범위:** 기업 정책에서 조직으로 모델 관리 권한을 위임하거나, 기업 팀 모드에서 팀·앱으로 위임하는 구조를 지원합니다.<a href="#source-2">[2]</a>
- **의미 — 해석:** 앞선 모델 종료가 ‘무엇을 계속 쓸 수 있는가’의 변화라면, 이번 발표는 ‘누가 어떤 범위에서 사용 가능 모델을 관리하는가’의 변화입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
  API 제공만으로 대체 모델이 자동 활성화되거나 고객 설정이 변경되는 것은 아닙니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

<p class="news-source"><a href="https://github.blog/changelog/2026-10-19-copilot-model-policy-api-for-enterprise-orgs-and-teams">GitHub · Copilot 모델 정책 API 정식 제공</a><br><span class="source-published">게시: 2026-10-20 06:32 KST</span><br><span class="source-modified">수정: 2026-10-20 07:00 KST</span></p>

## AI 트렌드와 변화 신호

- **관찰 — 모델 수명주기와 사용 통제:** 같은 날 모델 종료 공지와 정책 관리 API가 나왔습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
  이번 자료에서는 모델 선택뿐 아니라 조직별 사용 허용·관리 경로가 함께 중요해졌다는 신호를 읽을 수 있습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- **전날 대비:** 전날 보고서의 공익 기금·Beam·Hy-Memory 공개와는 다른 사건인 Copilot 지원 종료 시행과 정책 API 제공을 채택했습니다. 시장 전체의 관심도 상승·하락을 측정한 결과는 아닙니다.

## 조사 한계

- 최근 24시간을 우선 조사했으며, 새 실질 변화를 공식 원문으로 확인한 두 건만 실었습니다. 모델·제품, 에이전트·개발 도구, 규제·정책, 투자·파트너십을 살폈지만 전 분야를 망라한 목록은 아닙니다.
- 두 뉴스는 GitHub의 공식 제공·지원 상태 발표를 확인한 결과입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a> 실제 고객 계정의 모델 선택 화면이나 정책 변경 동작은 시험하지 않았습니다.
- 지원 종료의 정확한 적용 시각과 이용 환경별 완료 시점은 원문에서 확인하지 못했습니다.<a href="#source-1">[1]</a>

## Sources

[1] <span id="source-1"></span> [GitHub · Copilot 6개 모델 지원 종료](https://github.blog/changelog/2026-10-19-github-copilot-models-deprecated-on-october-19-2026)

[2] <span id="source-2"></span> [GitHub · Copilot 모델 정책 API 정식 제공](https://github.blog/changelog/2026-10-19-copilot-model-policy-api-for-enterprise-orgs-and-teams)
