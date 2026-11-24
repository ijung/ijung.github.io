---
title: "AI 데일리 리포트 — 2026-11-25"
date: "2026-11-25 06:00:41 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-25/
---

검수 기준: 2026-11-25 06:10 KST

## 30초 브리핑

- Anthropic의 Claude Ship은 코드 작성 이후의 검토·수정·정책 기반 승인까지 묶어 제공합니다. 새 수정·승인 에이전트는 베타입니다.<a href="#source-1">[1]</a>
- OpenAI와 Cisco가 Linux 서비스의 재시작 없이 임시 방어 규칙을 적용하는 오픈소스 Shirudo를 발표했습니다.<a href="#source-2">[2]</a>
- Google 검색 AI Mode의 Wallet 연결이 선택 동의 방식으로 배포되기 시작했습니다. Drive·Gemini 앱 연결은 향후 수주 내 예정입니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### Claude Ship, 코드 검토에서 조건부 승인까지 연결

**Anthropic은 Claude Team·Enterprise의 GitHub 저장소를 대상으로 Claude Ship을 제공하며, 기존 Code Review에 베타 수정·승인 에이전트를 더했습니다.**<a href="#source-1">[1]</a>

- **달라진 점:** <span id="def-steward">PR Steward는 자동 검사 실패·코드 충돌·검토 의견을 수정하는 에이전트이며, 직접 승인하거나 병합하도록 설계되지는 않았습니다.</span><a href="#source-1">[1]</a> <span id="def-approvals">Claude Approvals는 팀이 저장소 파일에 적은 정책에 따라 일상적인 변경의 승인 여부를 판단하는 에이전트입니다.</span><a href="#source-1">[1]</a>
- **왜 중요한지:** 코드를 더 많이 만드는 것뿐 아니라, 어떤 변경을 사람에게 넘길지 정책으로 정하는 단계까지 자동화 범위가 넓어졌습니다.<a href="#source-1">[1]</a> 다만 기존 브랜치 보호·필수 검사는 유지되며, 회사는 우선 실제 승인 없이 판단만 관찰하는 방식으로 시작할 것을 권고합니다.<a href="#source-1">[1]</a>

사용량에 따라 추가 비용이 발생하며 관찰 모드도 완전 무료는 아닙니다.<a href="#source-1">[1]</a> 제공 제외 설정은 Zero Data Retention·HIPAA 구성·고객 관리 암호화 키입니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://claude.com/resources/articles/introducing-claude-ship">Anthropic · Introducing Claude Ship</a><br><span class="source-published">게시: 2026-11-23 · 시각 미공개</span></p>

### Shirudo, 패치를 기다리는 Linux에 임시 방어막 적용

**OpenAI와 Cisco는 운영자가 승인한 임시 방어 규칙을 실행 중인 Linux에 적용하는 오픈소스 보안 실행 환경 Shirudo를 공개했습니다.**<a href="#source-2">[2]</a>

- **달라진 점:** <span id="def-shield">Shirudo의 shield는 정식 패치 전까지 취약점을 악용하는 동작을 막기 위한 임시 보호 규칙입니다.</span><a href="#source-2">[2]</a> 적용 전 정상 요청에 미칠 영향을 관찰할 수 있고, 실제 배포 여부는 보안 담당자가 통제합니다.<a href="#source-2">[2]</a>
- **왜 중요한지:** 취약점 발견과 정식 패치 배포 사이를 보완하는 도구라는 의미가 있습니다.<a href="#source-2">[2]</a> 회사는 Codex가 만든 규칙으로 실행 중인 Elasticsearch의 Java4Shell 공격 시도를 막은 통제 실험을 제시했지만, 모든 공격이나 운영 환경에서의 효과를 검증한 결과는 아닙니다.<a href="#source-2">[2]</a>

<span id="def-ebpf">eBPF는 Linux 운영체제 내부의 특정 지점에서 작은 프로그램을 실행하게 하는 기술입니다.</span><a href="#source-2">[2]</a> Shirudo는 이를 이용해 동작을 관찰하고 보호 규칙을 적용하며, 임시 방어는 정식 패치를 준비·배포하는 동안 사용하는 수단입니다.<a href="#source-2">[2]</a>

<p class="news-source"><a href="https://openai.com/index/introducing-shirudo">OpenAI · OpenAI and Cisco introduce Shirudo</a><br><span class="source-published">게시: 2026-11-23 · 시각 미공개</span></p>

### Google AI Mode, Wallet 연결 시작…돈을 움직이는 권한은 제외

**Google은 Wallet 정보를 검색 답변의 맥락으로 쓰는 연결을 순차 배포하며, Drive와 Gemini 앱 연결은 향후 수주 내 지원한다고 발표했습니다.**<a href="#source-3">[3]</a>

<span id="def-personal">Personal Intelligence는 사용자가 연결에 동의한 앱의 개인 맥락을 활용해 답변을 맞춤화하는 기능입니다.</span><a href="#source-3">[3]</a>

- **달라진 점:** 기존 Gmail·Calendar·Photos에 이어 지출·문서·대화의 맥락으로 연결 범위를 넓힙니다.<a href="#source-3">[3]</a> 다만 Wallet의 배포 시작과 Drive·Gemini의 예정 상태는 다릅니다.<a href="#source-3">[3]</a>
- **왜 중요한지:** 검색이 공개 웹 정보만이 아니라 개인 계정 정보도 참고하는 방향의 변화입니다.<a href="#source-3">[3]</a> Wallet 연결은 읽기 전용으로, AI Mode가 돈을 이동·관리·투자할 수는 없으며 사용자가 연결을 끌 수 있습니다.<a href="#source-3">[3]</a>

한국 제공 범위와 계정별 실제 활성화 여부는 이번 조사에서 확인하지 못했습니다.

<p class="news-source"><a href="https://blog.google/products-and-platforms/products/search/google-search-personal-intelligence-apps">Google · A more tailored Google Search that’s uniquely yours</a><br><span class="source-published">게시: 2026-11-24 02:00 KST</span><br><span class="source-modified">수정: 2026-11-24 02:43 KST</span></p>

## 기타 뉴스

<ul>
<li><strong>GPT-6 단백질 설계, 실험 결과와 전문 모델 의존성 함께 공개</strong><br>OpenAI는 GPT-6.2의 구조 설계를 바탕으로 만든 작은 단백질 322개 중 35개가 목표에 결합했다고 발표했습니다.<a href="#source-4">[4]</a> 다만 실험용 서열 생성·후보 선별에는 전문 모델을 사용했으며, 결합 확인은 약효·안전성 입증과 다릅니다.<a href="#source-4">[4]</a>
<p class="news-source"><a href="https://openai.com/index/gpt-6-models-demonstrate-intrinsic-protein-design-capabilities">OpenAI · GPT-6 models demonstrate intrinsic protein-design capabilities</a><br><span class="source-published">게시: 2026-11-24 · 시각 미공개</span></p>
</li>
<li><strong>DeepMind, 양자 알고리즘 개선에 수학적 검증 결합</strong><br>Gemini 4 에이전트가 제안한 개선을 Lean으로 검증하고 관련 라이브러리를 공개한다고 발표했습니다.<a href="#source-5">[5]</a> <span id="def-formal">형식 검증은 여기서 알고리즘의 수학적 정당성을 정리 증명기로 확인하는 방식입니다.</span><a href="#source-5">[5]</a> 실제 대규모 양자 하드웨어에서의 실행 속도를 측정한 결과는 아닙니다.<a href="#source-5">[5]</a>
<p class="news-source"><a href="https://deepmind.google/blog/finding-faster-quantum-algorithms-with-agents-and-formal-verification">Google DeepMind · Finding faster quantum algorithms with agents and formal verification</a><br><span class="source-published">게시: 2026-11-24 02:00 KST</span><br><span class="source-modified">수정: 2026-11-24 01:59 KST</span></p>
</li>
<li><strong>Amazon, Decart 최적화 기술·인력 확보 계약</strong><br>Amazon은 Decart Optimization Stack의 독점 기술을 구매하고 창업자·연구진을 영입하는 계약을 체결했습니다.<a href="#source-6">[6]</a> 회사 전체 인수 완료 발표가 아니며, 규제 승인 등 거래 종결 조건이 남아 있습니다.<a href="#source-6">[6]</a>
<p class="news-source"><a href="https://www.aboutamazon.com/news/company-news/amazon-decart-ai-optimization">Amazon · Amazon to purchase Decart&#x27;s AI optimization technology</a><br><span class="source-published">게시: 2026-11-23 22:46 KST</span></p>
</li>
<li><strong>한국 AI 예산 정부안, 약 16조 원과 물류 로봇 실증 방향 제시</strong><br>KBS는 11월 24일 K-AX 데이에서 정부 관계자가 2027년 AI 예산 정부안을 약 16조 원으로 설명하고 우정 물류 로봇 실증 방향을 제시했다고 보도했습니다.<a href="#source-7">[7]</a> 국회 확정 예산이나 이미 시작된 사업으로 해석해서는 안 됩니다.<a href="#source-7">[7]</a>
<p class="news-source"><a href="https://news.kbs.co.kr/news/view.do?ncd=8721166">KBS · 과기부 “내년 AI 정부 예산 16조 원…풀스택 수출 본격화 목표”</a><br><span class="source-published">게시: 2026-11-24 17:54 (원문 표기 · 시간대 확인 필요)</span><br><span class="source-modified">수정: 2026-11-24 18:01 (원문 표기 · 시간대 확인 필요)</span></p>
</li>
</ul>

## AI 트렌드와 변화 신호

- **권한을 어디까지 줄 것인가:** 코드 승인 정책, 운영자 승인형 방어 규칙, 읽기 전용 계정 연결이 함께 등장했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a> 이번에 선별한 사례에서는 기능 확대와 함께 권한 경계가 구체화됐다는 공통점이 있습니다.
- **제안과 검증의 분리:** 단백질 설계는 실험·전문 모델의 선별을 거쳤고, 양자 알고리즘은 수학적 증명을 사용했습니다.<a href="#source-4">[4]</a><a href="#source-5">[5]</a> AI가 제안했다는 사실과 실제 효용이 검증됐다는 결론은 구분해야 합니다.
- **전날 대비:** 전날 다룬 GLM-5.4와 AWS DevOps Agent의 동일 사건은 반복하지 않았습니다. 이번에는 코드 작성 이후의 승인, 임시 보안 방어, 개인 앱 연결의 실질 변화를 보완했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a> 시장 전체 관심도의 상승·하락을 측정한 결과는 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-25 06:00:41 KST입니다. 최근 24시간을 우선하되 최대 72시간 안의 미보도 중요 발표를 보완했습니다. 11월 23일 발표를 이번에 뒤늦게 확인한 항목이 있으며, 오늘 처음 발표된 소식으로 표시하지 않았습니다.
- 네 공식 목록을 직접 확인했지만 첫 화면 범위·월 단위 날짜·상단과 개별 항목의 날짜 차이로 전체 기간의 누락 여부는 확정하지 못했습니다. 추가로 발견한 일부 제품·정책 후보는 원문 확인 예산 안에서 검증하지 못해 제외했습니다.
- 기업 자료는 공식 발표·자체 실험의 근거이며 독립 성능 검증은 아닙니다. 국내 예산은 KBS의 행사 취재를 근거로 했으며 정부 예산 원안·국회 의결을 직접 대조하지 않았습니다. 보안 도구·계정 연결·모델의 실제 동작은 시험하지 않았습니다.
- 일부 보조 보도는 본문 추출로 읽었지만 별도 직접 접속은 차단됐습니다. 채택한 공식 자료와 KBS 본문은 직접 접속으로도 대조했습니다. 날짜만 공개된 자료의 시각과 시간대가 명시되지 않은 원문의 시간대는 추정하지 않았습니다. DeepMind의 수정 메타데이터는 게시보다 앞서 있어 갱신 순서는 추가 확인이 필요합니다. 필요한 용어는 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://claude.com/resources/articles/introducing-claude-ship">Anthropic · Introducing Claude Ship</a></p>

<p id="source-2" markdown="0">[2] <a href="https://openai.com/index/introducing-shirudo">OpenAI · OpenAI and Cisco introduce Shirudo</a></p>

<p id="source-3" markdown="0">[3] <a href="https://blog.google/products-and-platforms/products/search/google-search-personal-intelligence-apps">Google · A more tailored Google Search that’s uniquely yours</a></p>

<p id="source-4" markdown="0">[4] <a href="https://openai.com/index/gpt-6-models-demonstrate-intrinsic-protein-design-capabilities">OpenAI · GPT-6 models demonstrate intrinsic protein-design capabilities</a></p>

<p id="source-5" markdown="0">[5] <a href="https://deepmind.google/blog/finding-faster-quantum-algorithms-with-agents-and-formal-verification">Google DeepMind · Finding faster quantum algorithms with agents and formal verification</a></p>

<p id="source-6" markdown="0">[6] <a href="https://www.aboutamazon.com/news/company-news/amazon-decart-ai-optimization">Amazon · Amazon to purchase Decart&#x27;s AI optimization technology</a></p>

<p id="source-7" markdown="0">[7] <a href="https://news.kbs.co.kr/news/view.do?ncd=8721166">KBS · 과기부 “내년 AI 정부 예산 16조 원…풀스택 수출 본격화 목표”</a></p>
