---
title: "AI 데일리 리포트 — 2026-11-19"
date: "2026-11-19 06:00:25 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-19/
---

검수 기준: 2026-11-19 06:08 KST

## 30초 브리핑

- Microsoft의 AI 에이전트 개발 패키지에서 악성 버전이 발견됐으며, 해당 환경에서는 패키지를 불러오지 않아도 Python 시작만으로 정보 탈취 코드가 실행됩니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- OpenAI가 AMD 칩 설계에 모델의 회로 수정안이 반영됐다고 공개했습니다. 완제품 전체의 성능 개선과는 구분해야 합니다.<a href="#source-3">[3]</a>
- Copilot 앱의 Azure DevOps 연동 미리보기와 Enterprise Server 정식 지원을 확인했습니다. 전날 빠진 11월 17일 공지를 보완합니다.<a href="#source-4">[4]</a>

## 오늘의 주요 AI 뉴스

### Microsoft AI 개발 패키지 변조, Python 시작만으로 정보 탈취

**Microsoft Agent Framework의 배포 패키지 `agent-framework` 1.26.1·1.26.2에서 정보 탈취 코드가 발견됐습니다.**<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

<p id="def-supply-chain">공급망 공격은 이번 사례처럼 정상 소프트웨어의 배포 경로를 악용해 이용자 환경에 악성 코드를 전달하는 공격입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a></p>

- **달라진 점:** Wiz에 따르면 악성 버전은 11월 18일 05:35·06:08 KST에 게시됐고, 06:30 KST 기준 PyPI에서 내려갔습니다.<a href="#source-1">[1]</a>
  설치된 환경에서는 Python이 시작할 때 자동 실행되므로, 해당 패키지를 직접 불러오지 않았다는 이유만으로 안전하다고 판단할 수 없습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- **왜 중요한지:** AI 에이전트 기능 자체가 아니라 개발 도구의 설치·배포 경로가 위험의 출발점입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
  보안업체 분석과 전문매체 보도는 영향 환경의 격리·깨끗한 환경 재구축·노출 가능 자격증명 교체를 권고합니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
  실제 피해 범위와 최초 침해 경로를 이 보고서에서 확정하지는 않습니다.

<p class="news-source"><a href="https://www.wiz.io/blog/malicious-version-of-microsofts-agent-framework-published-to-pypi">Wiz · Microsoft agent-framework Compromised: Credential Stealer Targets Azure and CI Pipelines</a><br><span class="source-published">게시: 2026-11-18 07:03 KST</span><br><span class="source-modified">수정: 2026-11-18 23:00 KST</span></p>

<p class="news-source"><a href="https://www.bleepingcomputer.com/news/security/microsofts-agent-framework-pypi-package-compromised-to-steal-credentials">BleepingComputer · Microsoft’s Agent Framework PyPI package compromised to steal credentials</a><br><span class="source-published">게시: 2026-11-18 11:58 AM · 시간대 확인 필요</span></p>

### OpenAI, AMD 칩의 생산 설계에 AI 수정안 반영 공개

**OpenAI가 모델이 제안한 회로 변경 일부를 AMD가 제조에 넘긴 설계에 반영했다고 밝혔습니다.**<a href="#source-3">[3]</a>

<p id="def-rtl">RTL은 칩 회로의 동작을 코드로 표현하는 방식이며, 이번 모델은 이 코드를 다시 작성해 설계를 개선했습니다.<a href="#source-3">[3]</a></p>

- **달라진 점:** 자체 칩 사례를 넘어 AMD 설계에서의 적용 내용과 공개 프로세서 설계 평가 방법을 소개했습니다.<a href="#source-3">[3]</a>
  Synopsys 도구로 회로 동작 보존 여부와 소비전력·성능·면적의 절충을 확인했다고 설명했습니다.<a href="#source-3">[3]</a>
- **왜 중요한지:** 코드 작성 AI의 적용 범위가 소프트웨어에서 칩 회로 설계로 넓어진 구체적 사례입니다.<a href="#source-3">[3]</a>
  다만 공급자의 자체 성과 발표이며, 일부 회로 블록의 개선을 GPU 완제품 전체의 성능·전력 개선율로 읽어서는 안 됩니다.<a href="#source-3">[3]</a>

<p class="news-source"><a href="https://openai.com/index/advancing-chip-design-with-our-models">OpenAI · Advancing chip design with OpenAI models</a><br><span class="source-published">게시: 2026-11-18 · 시각 미공개</span></p>

### Copilot 앱, Azure DevOps 미리보기·Enterprise Server 정식 지원

**GitHub가 Copilot 앱의 기업용 저장소 지원 범위를 넓혔습니다.**<a href="#source-4">[4]</a>

- **달라진 점:** Azure DevOps 저장소에서 코드 변경과 변경 요청 처리를 지원하는 공개 미리보기를 제공합니다.<a href="#source-4">[4]</a>
  실험 설정을 켜고 Microsoft 계정으로 연결해야 하며, Azure DevOps 저장소의 클라우드 세션은 지원하지 않습니다.<a href="#source-4">[4]</a>
- **이용 조건과 의미:** GitHub Enterprise Server 지원은 정식 제공으로 전환됐으며 앱 1.2.0 이상이 필요합니다.<a href="#source-4">[4]</a>
  이 경우에도 클라우드 세션은 제공되지 않습니다.<a href="#source-4">[4]</a>
  기존 기업 저장소를 Copilot 앱의 작업 흐름에 연결한다는 의미가 있으며, 브라우저 조작 기능은 전날 다뤄 중복 소개하지 않습니다.

GitHub 원문은 11월 17일 공지이며, 전날 보고서에서 빠진 저장소 연동 변화를 이번에 보완했습니다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://github.blog/changelog/2026-11-17-github-copilot-app-microsoft-ignite-releases">GitHub · GitHub Copilot app, Microsoft Ignite releases</a><br><span class="source-published">게시: 2026-11-18 03:03 KST</span><br><span class="source-modified">수정: 2026-11-18 03:51 KST</span></p>

## 기타 뉴스

- **Copilot Fusion, 웹·Visual Studio·모바일 베타로 확대:** 작업에 맞는 모델을 선택·조율하는 기능의 이용 범위가 넓어졌으며, 정식 모바일 앱 지원은 아직 예정 상태입니다.<a href="#source-5">[5]</a>
  11월 17일 공지의 미보도 내용을 보완했으며, 회사의 벤치마크 비용 우월성 수치를 실제 업무 비용으로 일반화하지 않았습니다.

  <p class="news-source"><a href="https://github.blog/changelog/2026-11-17-github-copilot-fusion-updates-more-availability-benchmarks-and-models">GitHub · GitHub Copilot Fusion updates: more availability, benchmarks, and models</a><br><span class="source-published">게시: 2026-11-18 03:04 KST</span><br><span class="source-modified">수정: 2026-11-18 03:38 KST</span></p>

- **Copilot 활동을 Agent 365에서 확인:** 기업이 여러 Copilot 클라이언트의 프롬프트·응답·도구 호출 등 감사 데이터를 Agent 365로 보낼 수 있게 됐습니다.<a href="#source-6">[6]</a>

  <p id="def-emu">이 기능의 이용 조건인 Enterprise Managed Users는 기업이 사용자 계정을 관리하는 방식이며, 이를 사용하는 GitHub Enterprise Cloud 고객이 대상입니다.<a href="#source-6">[6]</a></p>

  11월 17일 공지의 미보도 내용을 보완했습니다.<a href="#source-6">[6]</a>

  <p class="news-source"><a href="https://github.blog/changelog/2026-11-17-monitor-github-copilot-activity-through-microsoft-agent-365">GitHub · Monitor GitHub Copilot activity through Microsoft Agent 365</a><br><span class="source-published">게시: 2026-11-18 03:00 KST</span><br><span class="source-modified">수정: 2026-11-18 03:40 KST</span></p>

- **Le Figaro 기사, ChatGPT 요약·발췌에 연결하는 제휴:** OpenAI가 현재 기사와 과거 기사에 대한 제휴를 발표했으며, 요약·발췌에 원문 링크를 포함할 예정입니다.<a href="#source-7">[7]</a>
  제휴 발표와 모든 이용자에 대한 실제 제공 완료는 구분합니다.<a href="#source-7">[7]</a>

  <p class="news-source"><a href="https://openai.com/index/le-groupe-figaro-and-openai-announce-partnership">OpenAI · Le Groupe Figaro and OpenAI partner to bring Le Figaro journalism to ChatGPT</a><br><span class="source-published">게시: 2026-11-18 · 시각 미공개</span></p>

## AI 트렌드와 변화 신호

- **개발 도구의 확장과 운영 통제가 함께 중요해집니다:** Copilot의 기업 저장소 연동·활동 기록 기능과 패키지 변조 사건을 함께 보면, 실행 편의뿐 아니라 설치 경로와 권한·기록을 살펴야 한다는 의미가 있습니다.<a href="#source-4">[4]</a><a href="#source-6">[6]</a><a href="#source-1">[1]</a>
- **전날 대비, 새 모델보다 적용 범위와 안전 이슈를 추가했습니다:** 전날의 GPT-6.2·브라우저 조작 소식은 반복하지 않고, 칩 설계 적용과 패키지 보안 사건, 미보도 기업용 기능을 확인했습니다.<a href="#source-3">[3]</a><a href="#source-1">[1]</a><a href="#source-4">[4]</a>
  이는 이번에 확인한 발표 묶음의 변화이며 시장 전체 관심도가 상승·하락했다는 뜻은 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-19 06:00:25 KST입니다. 최근 24시간을 우선하고, 최대 72시간 안의 미보도 중요 발표를 보완했습니다. 발표 날짜만 공개된 자료는 정확한 발표 시각을 추정하지 않았습니다.
- 네 공식 목록을 직접 읽었지만 일부 목록의 월 단위 날짜·상단과 항목 날짜 불일치로 전체 기간의 누락 여부는 확정하지 못했습니다. 국내 정책·투자와 일부 추가 기능 후보는 최신 원자료 검증이 충분하지 않아 포함하지 않았습니다.
- OpenAI 원문과 BleepingComputer 보도는 웹 추출로 읽었으나 직접 HTTP 확인은 차단됐습니다. BleepingComputer의 게시 시각은 원문 시간대가 불명확해 KST로 바꾸지 않았습니다.
- 보안 사건은 Wiz의 직접 분석과 다른 보안업체들을 인용한 전문매체 보도를 대조했습니다. 제품의 계정별 실제 이용과 칩 성과를 직접 시험하지 않았으며, 이해에 필요한 용어는 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://www.wiz.io/blog/malicious-version-of-microsofts-agent-framework-published-to-pypi">Wiz · Microsoft agent-framework Compromised: Credential Stealer Targets Azure and CI Pipelines</a></p>

<p id="source-2" markdown="0">[2] <a href="https://www.bleepingcomputer.com/news/security/microsofts-agent-framework-pypi-package-compromised-to-steal-credentials">BleepingComputer · Microsoft’s Agent Framework PyPI package compromised to steal credentials</a></p>

<p id="source-3" markdown="0">[3] <a href="https://openai.com/index/advancing-chip-design-with-our-models">OpenAI · Advancing chip design with OpenAI models</a></p>

<p id="source-4" markdown="0">[4] <a href="https://github.blog/changelog/2026-11-17-github-copilot-app-microsoft-ignite-releases">GitHub · GitHub Copilot app, Microsoft Ignite releases</a></p>

<p id="source-5" markdown="0">[5] <a href="https://github.blog/changelog/2026-11-17-github-copilot-fusion-updates-more-availability-benchmarks-and-models">GitHub · GitHub Copilot Fusion updates: more availability, benchmarks, and models</a></p>

<p id="source-6" markdown="0">[6] <a href="https://github.blog/changelog/2026-11-17-monitor-github-copilot-activity-through-microsoft-agent-365">GitHub · Monitor GitHub Copilot activity through Microsoft Agent 365</a></p>

<p id="source-7" markdown="0">[7] <a href="https://openai.com/index/le-groupe-figaro-and-openai-announce-partnership">OpenAI · Le Groupe Figaro and OpenAI partner to bring Le Figaro journalism to ChatGPT</a></p>
