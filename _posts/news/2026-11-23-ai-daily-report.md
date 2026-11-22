---
title: "AI 데일리 리포트 — 2026-11-23"
date: "2026-11-23 06:00:19 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-23/
---

검수 기준: 2026-11-23 06:09 KST

## 30초 브리핑

- Together AI가 코딩 에이전트의 자격증명 노출로 저장된 고객 프롬프트가 유출됐다고 공개했습니다.<a href="#source-1">[1]</a>
- Replit이 사내망의 서비스에 연결하는 Tunnels의 기업용 조기 접근을 안내했습니다.<a href="#source-2">[2]</a>
- OpenAI가 내부 사전학습 평가 지표를 공개하고, 이를 활용한 공동 개발 한도를 제안했습니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### Together AI, 코딩 에이전트의 자격증명 노출과 프롬프트 유출 공개

**Together AI는 11월 21일 공지에서, 코딩 에이전트가 공개 코드 저장소에 올린 자격증명으로 외부인이 저장된 프롬프트 샘플을 내려받았다고 밝혔습니다.**<a href="#source-1">[1]</a>

- **달라진 점:** 자동 탐지가 노출을 발견하고 회사가 해당 커밋을 되돌렸지만, 노출 중에 이미 다운로드가 이뤄졌다는 설명입니다.<a href="#source-1">[1]</a> 대상은 2026년 6~7월 요청 중 테스트용으로 보관한 일부 샘플이며, 공지에는 전체 피해 고객·프롬프트 수가 제시되지 않았습니다.<a href="#source-1">[1]</a>
- **왜 중요한지:** 코드의 비밀정보 탐지와 실제 접근 차단은 별개라는 사례입니다.<a href="#source-1">[1]</a> 회사는 제한된 권한·짧은 유효기간의 자격증명, 코드 변경 요청의 자격증명 검사, 코딩 에이전트 개발 절차 보강을 발표했습니다.<a href="#source-1">[1]</a>

<p id="def-zdr">제로 데이터 보관(ZDR)은 요청 데이터를 보관하지 않는 설정입니다.<a href="#source-1">[1]</a> Together AI는 이를 신규 계정의 기본값으로 바꿨다고 안내했으며, 기존 계정 전체에 자동 적용됐다는 의미는 아닙니다.<a href="#source-1">[1]</a></p>

회사의 사고 공지 기준이며, 노출된 내용과 고객 영향에 대한 검토는 진행 중입니다.<a href="#source-1">[1]</a> 유출 발생일 자체는 이번 공지에서 확인되지 않아, 공지일과 대상 요청의 생성 기간을 구분했습니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://www.together.ai/blog/security-event-prompt-leakage-for-requests-in-june-and-july-of-2026">Together AI · Security event: Prompt leakage for requests in June and July of 2026</a><br><span class="source-published">게시: 2026-11-21 · 시각 미공개</span><br><span class="source-modified">수정: 2026-11-22 14:56 KST</span></p>

### Replit Tunnels, 사내망 연결을 기업용 조기 접근으로 제공

**Replit은 11월 20일 기업이 사내망에서 실행하는 클라이언트를 통해 개발 환경을 내부 데이터베이스·서비스와 연결하는 Tunnels를 소개했습니다.**<a href="#source-2">[2]</a>

<p id="def-tunnel">이 기능의 터널은 사내망 안의 클라이언트를 거쳐 Replit 환경과 내부 서비스를 연결하는 통로입니다.<a href="#source-2">[2]</a> IT 관리자가 작업 공간별로 접근할 서비스를 정하며, 서비스를 공개 인터넷의 수신 연결에 노출하지 않는 방식입니다.<a href="#source-2">[2]</a></p>

- **달라진 점:** 연결 범위에는 에이전트의 도구 호출뿐 아니라 연결 도구의 인증 요청과 게시된 앱의 요청도 포함됩니다.<a href="#source-2">[2]</a> 현재 기업용 조기 접근 문의 단계이며, 전체 사용자 일반 제공으로 발표된 것은 아닙니다.<a href="#source-2">[2]</a>
- **왜 중요한지:** AI 개발 도구가 실제 사내 데이터에 접근할 경로를 제공하지만, 연결 자체가 데이터 접근 권한까지 대신하지는 않습니다.<a href="#source-2">[2]</a> 회사는 터널 구간의 암호화·인증과 별도로 개별 서비스의 인증·보안 연결을 설정해야 한다고 명시했습니다.<a href="#source-2">[2]</a>

기업의 제품 소개 자료이며, 보안 효과나 실제 업무 완료율을 독립 시험한 결과는 아닙니다.

<p class="news-source"><a href="https://replit.com/blog/build-with-private-data-using-tunnels">Replit · Securely connect Replit to your private network with Tunnels</a><br><span class="source-published">게시: 2026-11-21 02:55 KST</span><br><span class="source-modified">수정: 2026-11-21 12:46 KST</span></p>

### OpenAI, 내부 학습 지표 공개와 공동 개발 한도 제안

**OpenAI 수석과학자 Jakub Pachocki는 11월 22일 글에서 내부 사전학습 지표를 공개하고, 업계가 공통 지표와 안전 조건에 기반한 개발 한도를 정하자고 제안했습니다.**<a href="#source-3">[3]</a>

<p id="def-perplexity">퍼플렉서티(perplexity)는 모델이 다음 텍스트 조각을 얼마나 잘 예측하는지 나타내는 지표로, 낮을수록 예측이 더 정확합니다.<a href="#source-3">[3]</a> 이번 글은 이를 개발사 간 비교와 공동 안전 기준에 활용하자는 제안입니다.<a href="#source-3">[3]</a></p>

- **달라진 점:** OpenAI가 내부 코드의 고정된 평가 자료에서 추적해 온 측정치를 공유했습니다.<a href="#source-3">[3]</a> 새로운 모델의 이용 가능 범위나 가격을 발표한 글은 아닙니다.<a href="#source-3">[3]</a>
- **왜 중요한지:** 어떤 성능을 공개할지뿐 아니라, 어떤 안전 조건을 갖추기 전 개발을 제한할지까지 논의 대상으로 제시했다는 의미가 있습니다.<a href="#source-3">[3]</a> 다만 개발사마다 평가 데이터가 다르고, 이 지표가 실제 작업 능력이나 추론 기법의 효과를 모두 반영하지 못한다는 한계도 글에 명시됐습니다.<a href="#source-3">[3]</a>

공식 연구·의견 글이며, 업계 합의나 시행 중인 규제로 확정된 내용은 아닙니다.<a href="#source-3">[3]</a>

<p class="news-source"><a href="https://openai.com/index/perplexity-and-intelligence">OpenAI · Perplexity and Intelligence</a><br><span class="source-published">게시: 2026-11-22 · 시각 미공개</span></p>

## AI 트렌드와 변화 신호

- **접속 가능성과 안전한 위임의 구분:** Together AI의 유출 공지와 Replit의 사내망 연결은, 에이전트가 어디까지 접근하고 어떤 데이터를 남기는지 확인하는 일이 기능 도입과 분리될 수 없다는 신호입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- **평가 지표의 공개에서 개발 조건 논의로:** OpenAI의 제안은 성능 경쟁의 지표를 공동 안전 기준에도 쓰려는 시도입니다.<a href="#source-3">[3]</a> 실제 채택 여부는 추가 확인이 필요합니다.
- **전날 대비:** 전날 보고서의 로컬 모델·Mac 앱 조작·음성 개발 기능을 반복하지 않고, 이번에는 사고 공개·기업용 연결 기능·평가 지표 공개를 선별했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a> 시장 전체 관심도의 상승·하락을 측정한 결과는 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-23 06:00:19 KST입니다. 최근 24시간을 우선하고 최대 72시간 안의 미보도 발표를 확인했습니다. Replit의 11월 20일 발표는 뒤늦게 보완한 내용이며, 오늘 새 발표로 표시하지 않았습니다.
- 네 공식 목록을 직접 읽었지만 첫 화면 범위·월 단위 날짜·상단과 개별 항목의 날짜 차이 때문에 전체 기간의 누락 여부는 확정하지 못했습니다. 추가 모델·투자·정책 후보는 최신성이나 원문 근거가 부족해 제외했습니다.
- Together AI와 OpenAI의 원문은 게시 날짜만 확인됐으며, 게시 시각은 공개되지 않았습니다. 회사 발표의 성능·보안 효과와 계정별 실제 제공 상태는 직접 시험하지 않았으며, 필수 용어는 관련 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://www.together.ai/blog/security-event-prompt-leakage-for-requests-in-june-and-july-of-2026">Together AI · Security event: Prompt leakage for requests in June and July of 2026</a></p>

<p id="source-2" markdown="0">[2] <a href="https://replit.com/blog/build-with-private-data-using-tunnels">Replit · Securely connect Replit to your private network with Tunnels</a></p>

<p id="source-3" markdown="0">[3] <a href="https://openai.com/index/perplexity-and-intelligence">OpenAI · Perplexity and Intelligence</a></p>
