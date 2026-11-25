---
title: "AI 데일리 리포트 — 2026-11-26"
date: "2026-11-26 06:00:35 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-26/
---

검수 기준: 2026-11-26 06:04 KST

## 30초 브리핑

- Claude의 외부 도구 연결에 작업 지침·백그라운드 실행·중간 확인 기능이 더해졌습니다. 기능별 베타·정식 제공·예정 상태는 다릅니다.<a href="#source-1">[1]</a>
- Claude에서 만든 작업물을 Mac·Windows 앱으로 설치하는 기능이 베타 배포됩니다. 컴퓨터 전체 접근은 별도 허가가 필요합니다.<a href="#source-2">[2]</a>
- PrismML은 기기 안에서 여러 단계의 도구 작업을 수행하도록 개선한 Bonsai 2.5의 모델 가중치를 공개했습니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### Claude의 MCP 연결, 도구 호출에서 업무 지침·중간 확인까지 확대

**Anthropic은 Skills over MCP를 공개 베타로 제공하고, Tasks와 Elicitation의 정식 제공을 발표했습니다.**<a href="#source-1">[1]</a>

<span id="def-mcp">MCP는 AI 에이전트를 데이터와 도구가 있는 외부 시스템에 연결하는 공개 표준입니다.</span><a href="#source-1">[1]</a> <span id="def-skills">Skills over MCP는 서버가 도구와 함께 업무 지식·작업 절차를 전달하고, Claude가 필요할 때 불러오는 기능입니다.</span><a href="#source-1">[1]</a>

- **달라진 점:** <span id="def-tasks">Tasks는 서버에 오래 걸리는 작업을 맡긴 동안 Claude가 다른 일을 계속하고 나중에 결과를 받는 기능입니다.</span><a href="#source-1">[1]</a> <span id="def-elicitation">Elicitation은 서버가 작업 도중 양식이나 링크로 사용자 입력·확인을 요청한 뒤 이어서 진행하는 기능입니다.</span><a href="#source-1">[1]</a>
- **왜 중요한지:** 외부 도구의 사용법과 실행·확인 과정을 하나의 연결에서 다룬다는 의미가 있습니다.<a href="#source-1">[1]</a> 다만 서버와 클라이언트가 해당 기능을 지원해야 합니다.<a href="#source-1">[1]</a>

Skills는 Pro·Max·Ultra에서 기본 활성화되며 Team은 관리자 설정이 필요하고, Enterprise는 예정입니다.<a href="#source-1">[1]</a> Tasks·Elicitation은 Pro·Max·Ultra에 정식 제공되며 Team·Enterprise에는 순차 배포됩니다.<a href="#source-1">[1]</a>

<span id="def-files">File Uploads는 파일을 대화 문맥에 넣지 않고 MCP 서버로 전달하는 확장 기능입니다.</span><a href="#source-1">[1]</a> 이 기능은 아직 배포 예정입니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://claude.com/resources/articles/skills-over-mcp-tasks-and-more">Anthropic · MCP servers can now ship skills, run tasks, and more with Claude</a><br><span class="source-published">게시: 2026-11-24 · 시각 미공개</span></p>

### Claude 작업물을 데스크톱 앱으로 설치…허가 범위 확인이 핵심

**‘Install as app’이 모든 요금제의 Mac·Windows용 Claude에 베타로 순차 배포됩니다.**<a href="#source-2">[2]</a>

<span id="def-artifacts">Artifacts는 Claude에서 만든 차트·웹페이지 같은 작업물이며, 이번 기능은 이를 독립 창과 아이콘을 가진 앱으로 설치하게 합니다.</span><a href="#source-2">[2]</a>

- **달라진 점:** 설치한 앱은 알림·메뉴 막대 등을 활용하고, Claude와 연결된 외부 도구를 사용할 수 있습니다.<a href="#source-2">[2]</a> Claude에 보내는 요청은 기존 요금제 사용 한도에 포함됩니다.<a href="#source-2">[2]</a>
- **왜 중요한지:** 대화 안의 결과물을 반복 사용하는 업무 도구로 바꾸는 변화입니다.<a href="#source-2">[2]</a> 전체 컴퓨터 접근을 허가하면 매번 묻지 않고 프로그램 실행·파일 읽기·수정·삭제가 가능하므로 기본 브라우저 수준 권한과 구분해야 합니다.<a href="#source-2">[2]</a>

업데이트마다 권한을 다시 요청하지만, 앱에 포함된 외부 웹사이트 부분의 변경은 예외입니다.<a href="#source-2">[2]</a> 전체 접근은 직접 만든 앱 또는 조직 구성원이 만든 앱에만 허가할 수 있으며, Anthropic은 설치 전 앱을 심사하지 않습니다.<a href="#source-2">[2]</a>

Team은 기능이 기본 활성화되지만 전체 접근은 꺼져 있고, Enterprise는 소유자가 켜야 합니다.<a href="#source-2">[2]</a> HIPAA-ready 조직·하이브리드 배포는 제외되며, 앱 실행에도 인터넷과 Claude 로그인이 필요합니다.<a href="#source-2">[2]</a>

<p class="news-source"><a href="https://claude.com/resources/articles/install-as-app">Anthropic · Turn artifacts into Mac and Windows apps</a><br><span class="source-published">게시: 2026-11-24 · 시각 미공개</span></p>

### Bonsai 2.5 공개, 로컬 AI의 여러 단계 도구 사용에 초점

**PrismML은 Qwen3.8 27B 기반 Ternary Bonsai 2.5 27B의 가중치를 Apache 2.0 라이선스로 공개했습니다.**<a href="#source-3">[3]</a>

- **달라진 점:** 회사는 이전 Bonsai 2보다 도구 호출 결과를 해석하고 실패에 대응하는 능력을 개선했다고 발표했습니다.<a href="#source-3">[3]</a> 이는 회사 자체 평가이며 독립 재현 결과는 아닙니다.
- **왜 중요한지:** 민감한 파일을 외부로 보내지 않고 기기에서 처리하는 제품을 만들기 위한 선택지가 늘었다는 의미가 있습니다.<a href="#source-3">[3]</a> 다만 모델 크기를 곧바로 모든 기기의 실제 실행 메모리 요구량으로 읽어서는 안 됩니다.

공식 발표의 모델 크기는 5.9 GB이며, 9to5Mac은 가장 작은 형식의 크기라고 설명하면서 Apple silicon 패키지는 약 8.9 GB라고 보도했습니다.<a href="#source-3">[3]</a><a href="#source-4">[4]</a> 패키지·실행 환경의 차이를 구분해야 하며, 이번 조사에서는 실제 메모리 사용량이나 속도를 시험하지 않았습니다.

<p class="news-source"><a href="https://prismml.com/news/bonsai-2-5-27b">PrismML · Introducing Ternary Bonsai 2.5 27B: Powering Agentic Coding and Private Knowledge Work</a><br><span class="source-published">게시: 2026-11-24 · 시각 미공개</span><br><span class="source-modified">수정: 2026-11-25 04:14 KST</span></p>

<p class="news-source"><a href="https://9to5mac.com/2026/11/24/prismmls-bonsai-2-5-brings-better-coding-and-tool-use-to-on-device-ai">9to5Mac · PrismML’s Bonsai 2.5 brings better coding and tool use to on-device AI</a><br><span class="source-published">게시: 2026-11-25 10:08 KST</span></p>

## 기타 뉴스

<ul>
<li><strong>Google Antigravity, 분자 분석용 Mass Spec Skills 제공</strong><br><span id="def-massspec">Mass Spec Skills는 질량분석 도구와 공개 화학 데이터베이스를 연결해 분자 구조 후보와 검토 근거를 제시하는 전문 작업 기능 묶음입니다.</span><a href="#source-5">[5]</a> Google은 현재 사용 가능하다고 발표했지만, 데이터베이스에 없는 새 분자의 구조를 이 도구만으로 확정할 수는 없다고 밝혔습니다.<a href="#source-5">[5]</a>
<p class="news-source"><a href="https://deepmind.google/blog/interpreting-mass-spectra-with-google-antigravity">Google DeepMind · From spectrum to structure: Identifying molecules with Mass Spec Skills for Google Antigravity</a><br><span class="source-published">게시: 2026-11-26 02:00 KST</span><br><span class="source-modified">수정: 2026-11-26 02:09 KST</span></p>
</li>
<li><strong>영국 NCSC, AI 공격의 변화는 ‘새 취약점’보다 속도·지속성</strong><br>NCSC는 최근 관찰한 AI 지원 공격이 기존 설정 오류·노출된 자격증명 등을 더 빠르고 지속적으로 악용한다고 설명했습니다.<a href="#source-6">[6]</a> <span id="def-offensive">에이전트형 공격 프레임워크는 AI가 보안 도구를 실행하고 결과를 읽어 다음 공격 단계를 정하게 하는 소프트웨어입니다.</span><a href="#source-6">[6]</a> 관측 대상의 설명이며 모든 AI 공격에 대한 보편적 결론은 아닙니다.
<p class="news-source"><a href="https://www.ncsc.gov.uk/blogs/familiar-weaknesses-exploited-faster-defending-against-agentic-cyber-attacks">NCSC · Familiar weaknesses, exploited faster: defending against agentic cyber attacks</a><br><span class="source-published">게시: 2026-11-24 21:00 KST</span></p>
</li>
<li><strong>Anthropic, 고객 저장소 기반 안전 모니터링 공개 베타</strong><br><span id="def-efs">Enterprise Frontier Safeguards(EFS)는 고객이 통제하는 저장소의 데이터를 자동 분석해 여러 요청에 걸친 오용 패턴을 찾는 안전 모니터링 기능입니다.</span><a href="#source-7">[7]</a> <span id="def-zdr">이번 EFS의 Zero Data Retention(ZDR)은 입력·출력을 고객 저장소에 전달한 뒤 Anthropic이나 클라우드 제공자가 별도 사본을 보유하지 않는다는 조건입니다.</span><a href="#source-7">[7]</a> 고객 저장소에는 데이터가 남고 저장·읽기 등의 비용이 발생하며, Microsoft Foundry 지원은 예정입니다.<a href="#source-7">[7]</a>
<p class="news-source"><a href="https://claude.com/resources/articles/zero-data-retention-with-enterprise-frontier-safeguards">Anthropic · Zero data retention with Enterprise Frontier Safeguards is now in beta</a><br><span class="source-published">게시: 2026-11-23 · 시각 미공개</span></p>
</li>
</ul>

## AI 트렌드와 변화 신호

- **도구와 지침의 결합:** MCP 서버의 업무 지침 전달과 Antigravity의 전문 분석 기능은 모델 단독 추론보다 검증된 도구·절차를 연결하는 방식에 초점을 둡니다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a> 이번 사례에서 확인되는 제품 설계 방향이며 시장 전체의 우세를 측정한 결과는 아닙니다.
- **실행 위치와 권한을 함께 확인:** 데스크톱 앱의 전체 접근, 기기 내 모델 실행, 고객 저장소 기반 모니터링은 서로 다른 통제 방식입니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a><a href="#source-7">[7]</a> ‘로컬’이나 ‘데이터 미보관’이라는 이름만으로 권한·저장 범위가 같다고 볼 수는 없습니다.
- **전날 대비:** 전날 다룬 코드 승인·임시 보안 방어·개인 앱 연결과 별개로, 이번에는 MCP 작업 방식과 설치형 작업물의 제공 변화를 확인했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a> 관심도 상승·하락을 수치로 측정한 보고는 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-26 06:04:55 KST입니다. 최근 24시간을 우선하고 최대 72시간 안의 미보도 발표를 보완했습니다. 11월 23~24일 발표는 오늘 발표가 아니라 뒤늦게 확인한 소식입니다.
- 네 공식 목록을 직접 읽었지만 첫 화면 범위·월 단위 날짜·상단과 개별 항목의 날짜 차이 때문에 전체 기간의 누락 여부는 확정하지 못했습니다. 일부 모델·투자·정책 후보는 원자료나 사건 시점 확인이 부족해 제외했습니다.
- 제품 제공 상태·안전 설계·성능 개선은 공식 발표 기준이며 실제 제품 시험이나 독립 보안 감사 결과가 아닙니다. 한국 계정별 활성화 여부는 확인하지 못했습니다. 날짜만 공개된 원문의 시각은 추정하지 않았으며, 필요한 용어는 관련 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://claude.com/resources/articles/skills-over-mcp-tasks-and-more">Anthropic · MCP servers can now ship skills, run tasks, and more with Claude</a></p>

<p id="source-2" markdown="0">[2] <a href="https://claude.com/resources/articles/install-as-app">Anthropic · Turn artifacts into Mac and Windows apps</a></p>

<p id="source-3" markdown="0">[3] <a href="https://prismml.com/news/bonsai-2-5-27b">PrismML · Introducing Ternary Bonsai 2.5 27B: Powering Agentic Coding and Private Knowledge Work</a></p>

<p id="source-4" markdown="0">[4] <a href="https://9to5mac.com/2026/11/24/prismmls-bonsai-2-5-brings-better-coding-and-tool-use-to-on-device-ai">9to5Mac · PrismML’s Bonsai 2.5 brings better coding and tool use to on-device AI</a></p>

<p id="source-5" markdown="0">[5] <a href="https://deepmind.google/blog/interpreting-mass-spectra-with-google-antigravity">Google DeepMind · From spectrum to structure: Identifying molecules with Mass Spec Skills for Google Antigravity</a></p>

<p id="source-6" markdown="0">[6] <a href="https://www.ncsc.gov.uk/blogs/familiar-weaknesses-exploited-faster-defending-against-agentic-cyber-attacks">NCSC · Familiar weaknesses, exploited faster: defending against agentic cyber attacks</a></p>

<p id="source-7" markdown="0">[7] <a href="https://claude.com/resources/articles/zero-data-retention-with-enterprise-frontier-safeguards">Anthropic · Zero data retention with Enterprise Frontier Safeguards is now in beta</a></p>
