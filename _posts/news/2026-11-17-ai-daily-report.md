---
title: "AI 데일리 리포트 — 2026-11-17"
date: "2026-11-17 06:09:44 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-17/
---

검수 기준: 2026-11-17 06:09 KST

## 30초 브리핑

- OpenAI가 의료 소프트웨어를 직접 조작하는 에이전트와 20개 이상의 새 의료 플러그인을 발표했습니다.<a href="#source-1">[1]</a>
  실제 제공은 워크스페이스 설정에 따릅니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- Qwen4-Max가 QwenCloud와 업무용 앱에 제공됩니다.<a href="#source-3">[3]</a>
  작업 비용 절감 수치는 회사 자체 평가이며 실제 이용 비용과 구분해야 합니다.<a href="#source-3">[3]</a>
- Copilot에서 Opus 4.8·Opus 5·GPT-5.6 Terra의 지원 종료가 적용됐습니다.<a href="#source-4">[4]</a>
  연간 개인 구독과 내부 대체 동작에는 예외가 있습니다.<a href="#source-4">[4]</a>

## 오늘의 주요 AI 뉴스

### 의료 AI, 답변을 넘어 기존 업무 화면 조작으로 확대

**OpenAI가 의료용 컴퓨터 사용 에이전트, 임상 검색 개선, 20개 이상의 새 플러그인을 함께 발표했습니다.**<a href="#source-1">[1]</a>

<p id="def-computer-use">컴퓨터 사용 에이전트는 화면에서 클릭·입력·앱 이동을 수행해 여러 단계의 업무를 처리하는 AI입니다.<a href="#source-2">[2]</a></p>

- **달라진 점:** 의료 기록 검토와 처방·환자 답변 초안 작성, 보험 승인 요청 준비 등 기존 소프트웨어 사이의 작업을 지원합니다.<a href="#source-2">[2]</a>
  OpenAI API·ChatGPT for Healthcare·ChatGPT for Clinicians에서 제공한다고 안내했습니다.<a href="#source-2">[2]</a>
- **왜 중요한지:** 자료를 찾아 답하는 기능과 실제 업무 단계를 실행하는 기능을 연결했다는 의미가 있습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
  조직은 접근할 앱과 사람의 검토가 필요한 단계를 정해야 합니다.<a href="#source-1">[1]</a>
- **이용 조건:** Healthcare의 새 기능은 워크스페이스 설정에 따르며 플러그인은 관리자 활성화가 필요합니다.<a href="#source-1">[1]</a>
  무료 Clinicians는 자격을 확인한 미국 임상의 대상입니다.<a href="#source-1">[1]</a>
  국내 제공 여부는 추가 확인이 필요합니다.

<p class="news-source"><a href="https://openai.com/index/more-ways-to-move-care-forward-with-chatgpt-for-healthcare">OpenAI · More ways to move care forward with ChatGPT for Healthcare</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

<p class="news-source"><a href="https://openai.com/index/advancing-computer-use-for-healthcare-teams">OpenAI · Advancing computer-use agents for healthcare teams</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

### Qwen4-Max 제공 시작, 비용 수치는 자체 업무 평가 기준

**Qwen이 전문 업무와 장시간 작업 수행을 겨냥한 새 주력 모델 Qwen4-Max의 제공을 발표했습니다.**<a href="#source-3">[3]</a>

- **달라진 점:** QwenCloud에서 API 또는 Token Plan으로 이용할 수 있으며 QwenWork·Qoder·QwenStudio에서도 제공됩니다.<a href="#source-3">[3]</a>
  TNW는 11월 11일 제한적 미리보기 이후 출시됐으며, 모델을 내려받아 수정하는 방식은 제공하지 않았다고 보도했습니다.<a href="#source-5">[5]</a>
- **수치의 조건:** Qwen은 자체 Work360 평가에서 Qwen3.8-Max 대비 작업당 평균 비용이 4.43달러에서 1.67달러로 줄었다고 발표했습니다.<a href="#source-3">[3]</a>
  특정 평가의 평균 실행 비용이지 모든 업무의 고정 비용이나 API 단가가 아닙니다.<a href="#source-3">[3]</a>

<p id="def-work360">Work360은 Qwen이 산업·전문 분야의 복잡한 업무 수행을 평가하기 위해 만든 평가 체계입니다.<a href="#source-3">[3]</a>
따라서 이 비용 비교는 회사 자체 평가 결과로 읽어야 합니다.<a href="#source-3">[3]</a></p>

- **왜 중요한지:** 새 모델의 실제 이용 경로가 제시됐지만, 성능·비용 우월성은 독립 검증과 분리해서 판단해야 한다는 의미가 있습니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>
  공식 글 표시일은 11월 15일인 반면 TNW는 16일 출시로 보도해 발표일에 차이가 있습니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>
  이번 회차에 확인한 소식이며 오늘 출시로 단정하지 않습니다.

<p class="news-source"><a href="https://qwen.ai/blog?id=qwen4-max">Qwen · Qwen4-Max: Your New Teammate Has Arrived</a><br><span class="source-published">게시: 2026-11-15 · 시각 미공개</span></p>

<p class="news-source"><a href="https://thenextweb.com/news/alibaba-launches-qwen4-max-flagship-ai-model-without-open-weights">TNW · Alibaba launches Qwen4-Max flagship AI model without open weights</a><br><span class="source-published">게시: 2026-11-16 17:49 KST</span></p>

### Copilot 일부 모델 지원 종료 적용, 12월 예고와 별개

**GitHub가 11월 16일부터 Copilot의 Opus 4.8·Opus 5·GPT-5.6 Terra 지원 종료를 적용했다고 공지했습니다.**<a href="#source-4">[4]</a>

- **달라진 점:** 권장 대안은 Opus 계열의 경우 Opus 5.5, Terra의 경우 GPT-6.1 Sol 또는 GPT-6 Luna입니다.<a href="#source-4">[4]</a>
  전날 다룬 12월 지원 종료 예고와는 대상과 적용일이 다른 사건입니다.<a href="#source-4">[4]</a>
- **예외와 의미:** 연간 요금제의 개인 Copilot 구독자는 Opus 4.8을 계속 이용할 수 있습니다.<a href="#source-4">[4]</a>
  Opus 5도 선택 목록에서는 제거됐지만 기존 허용 접근의 실행 중 응답 거절에 대한 대체 모델 용도로는 남습니다.<a href="#source-4">[4]</a>
  일반 모델 선택 종료를 모든 내부 사용의 종료로 해석하면 안 됩니다.<a href="#source-4">[4]</a>
- **조직 이용:** 대체 모델의 자동 활성화에는 관리자 전역 기본값과 개별 모델 허용 설정이 적용됩니다.<a href="#source-4">[4]</a>
  기존 자동화에서 모델 이름만 교체하면 된다고 단정할 수 없습니다.<a href="#source-4">[4]</a>

<p class="news-source"><a href="https://github.blog/changelog/2026-11-16-github-copilot-models-deprecated-on-november-16-2026">GitHub · GitHub Copilot models deprecated on November 16, 2026</a><br><span class="source-published">게시: 2026-11-17 05:25 KST</span><br><span class="source-modified">수정: 2026-11-17 05:27 KST</span></p>

### 아동 성착취물 탐지 도구, 검증된 조직에 무료 제공

**OpenAI가 AI 생성물을 포함한 아동 성착취물 탐지를 돕는 새 이미지 분류기를 검증된 조직에 API로 무료 제공한다고 발표했습니다.**<a href="#source-6">[6]</a>

<p id="def-image-classifier">이번 이미지 분류기는 이미지의 학대 징후를 점수로 표시해 사람이 우선 검토할 대상을 고르도록 돕는 도구입니다.<a href="#source-6">[6]</a></p>

- **달라진 점:** 새로운 이미지의 시각적 징후를 탐지하며, 개발자용 대응 지침과 ChatGPT의 경고·이용 제한 조치도 함께 제시했습니다.<a href="#source-6">[6]</a>
- **왜 중요한지:** 모델의 유해 요청 거절을 넘어 다른 조직이 사용할 탐지 도구와 검토 절차로 대응 범위를 넓혔다는 의미가 있습니다.<a href="#source-6">[6]</a>
  점수는 검토 우선순위를 돕는 신호이며, 이번 보고서에서 탐지 정확도를 독립 검증한 것은 아닙니다.

<p class="news-source"><a href="https://openai.com/index/strengthening-online-child-safety">OpenAI · Strengthening the fight against online child sexual abuse with new tools and support</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

### OpenAI, 외부 평가기관의 내부 접근 계약 발표

**OpenAI가 Apollo Research·METR·Transluce·Goodfire와 내부 모델·시스템 평가를 위한 첫 단계 계약을 체결했다고 발표했습니다.**<a href="#source-7">[7]</a>

<p id="def-embedded-assessment">내부 접근형 평가(embedded assessments)는 외부 전문가가 연구소 내부 모델·시스템과 관련 자료에 접근해 일정 기간 안전장치를 평가하는 방식입니다.<a href="#source-7">[7]</a></p>

- **달라진 점:** 평가 필요에 따라 미공개 모델, 코드, 학습·평가 자료, 모니터링 기록 등에 접근하는 절차를 제시했습니다.<a href="#source-7">[7]</a>
  단순한 공개 모델 시험보다 내부 근거를 살필 수 있도록 한 조치라는 의미가 있습니다.<a href="#source-7">[7]</a>
- **주의할 점:** 계약 발표는 안전성 평가의 통과 결과가 아닙니다.<a href="#source-7">[7]</a>
  OpenAI는 기밀 삭제와 책임 있는 공개를 위한 지연 요청 권한을 유지합니다.<a href="#source-7">[7]</a>
  Apollo·Goodfire는 평가 결과에 연동되지 않는 보수를 받고, METR·Transluce는 평가 보수를 받지 않지만 API 크레딧·컴퓨팅 자원을 받을 수 있다고 공시했습니다.<a href="#source-7">[7]</a>

<p class="news-source"><a href="https://openai.com/index/our-first-phase-of-embedded-assessments">OpenAI · Our first phase of embedded assessments</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

## 기타 뉴스

- **연구자 무료 이용 지원, 추첨에서 선착순으로 전환:** OpenAI는 11월 23일 다음 신청을 열고 자격을 충족한 선착순 5만 건에 ChatGPT Plus를 제공할 계획입니다.<a href="#source-8">[8]</a>
  지원 한 건은 신청자와 최대 두 명의 협업자를 포함하며, 기존 시범사업의 Pro와는 다릅니다.<a href="#source-8">[8]</a>

  <p class="news-source"><a href="https://openai.com/index/expanding-chatgpt-for-academic-researchers">OpenAI · Broadening access to AI for scientists</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

- **AI 보안 검사, 저장소 최대 20개 동시 시작:** GitHub가 기본 브랜치의 취약점을 찾는 AI Scan의 일괄 시작 기능을 공개 미리보기로 제공했습니다.<a href="#source-9">[9]</a>
  Copilot 라이선스는 필요하지 않습니다.<a href="#source-9">[9]</a>

  <p class="news-source"><a href="https://github.blog/changelog/2026-11-16-bulk-start-ai-scan-for-repositories">GitHub · Bulk start AI Scan for repositories</a><br><span class="source-published">게시: 2026-11-17 00:50 KST</span><br><span class="source-modified">수정: 2026-11-17 00:54 KST</span></p>

- **LVMH 미디어 부문, OpenAI와 업무·콘텐츠 협력:** ChatGPT Enterprise의 사내 도입과 ChatGPT 답변의 기사 요약·발췌·원문 링크 제공을 포함한 계약을 발표했습니다.<a href="#source-10">[10]</a>
  OpenAI가 재게시한 파트너 보도자료 기준입니다.<a href="#source-10">[10]</a>

  <p class="news-source"><a href="https://openai.com/index/lvmh-media-division-partnership">OpenAI · LVMH’s Media Division announces strategic partnership with OpenAI</a><br><span class="source-published">게시: 2026-11-16 · 시각 미공개</span></p>

## AI 트렌드와 변화 신호

- **모델 발표에서 업무 연결로:** 의료 에이전트는 앱 사이의 실행을, Qwen4-Max는 업무용 제품에서의 이용 경로를 강조했습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>
  이번 발표 묶음에서는 모델 자체 성능 외에 실제 업무에 연결하는 방식이 관찰할 변화입니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>
- **전날 대비, 지원 종료의 단계 구분:** 전날 리포트는 12월 종료 예고를 다뤘고, 이번 공지는 다른 모델의 11월 종료 적용입니다.<a href="#source-4">[4]</a>
  예고·실제 적용·예외 사용을 구분해야 한다는 의미가 있습니다.<a href="#source-4">[4]</a>
- **안전성의 확인 방식:** 내부 접근형 외부 평가와 조직용 이미지 탐지 도구가 함께 발표됐습니다.<a href="#source-6">[6]</a><a href="#source-7">[7]</a>
  평가 접근권·공개 조건과 운영 도구의 검토 절차를 각각 살펴야 하며, 발표 자체를 효과 입증으로 볼 수는 없습니다.<a href="#source-6">[6]</a><a href="#source-7">[7]</a>

## 조사 한계

- 수집 기준은 2026-11-17 06:00:19 KST입니다. 최근 24시간을 우선하고 최대 72시간 안의 미보도 중요 발표를 확인했습니다. 전날 AI 전용 보고서와 사건 단위로 비교했습니다.
- 네 공식 목록을 직접 확인했지만 첫 화면·월 단위 날짜·상단과 항목 날짜의 차이로 전체 기간의 누락 여부를 확정하지 못했습니다. 국내 AI 기본법 관련 기사는 원문 추출이 차단돼 제외했습니다. 정책·투자 후보 중 원자료와 최신성을 충분히 대조하지 못한 항목도 제외했습니다.
- OpenAI 원문은 웹 추출로 읽었으나 직접 HTTP 검증은 차단됐습니다. 게시일만 공개된 자료에는 시각을 추정하지 않았습니다. Qwen 발표일의 차이는 위 기사에 별도로 표시했습니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a>
- 공급자의 제공·정책 발표 기준이며 실제 계정 이용, 의료 효과, 성능·탐지 정확도 시험을 수행하지 않았습니다. 의료 관련 두 발표는 하나의 뉴스로 묶었습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

## Sources

[1] <span id="source-1"></span> [OpenAI · More ways to move care forward with ChatGPT for Healthcare](https://openai.com/index/more-ways-to-move-care-forward-with-chatgpt-for-healthcare)

[2] <span id="source-2"></span> [OpenAI · Advancing computer-use agents for healthcare teams](https://openai.com/index/advancing-computer-use-for-healthcare-teams)

[3] <span id="source-3"></span> [Qwen · Qwen4-Max: Your New Teammate Has Arrived](https://qwen.ai/blog?id=qwen4-max)

[4] <span id="source-4"></span> [GitHub · GitHub Copilot models deprecated on November 16, 2026](https://github.blog/changelog/2026-11-16-github-copilot-models-deprecated-on-november-16-2026)

[5] <span id="source-5"></span> [TNW · Alibaba launches Qwen4-Max flagship AI model without open weights](https://thenextweb.com/news/alibaba-launches-qwen4-max-flagship-ai-model-without-open-weights)

[6] <span id="source-6"></span> [OpenAI · Strengthening the fight against online child sexual abuse with new tools and support](https://openai.com/index/strengthening-online-child-safety)

[7] <span id="source-7"></span> [OpenAI · Our first phase of embedded assessments](https://openai.com/index/our-first-phase-of-embedded-assessments)

[8] <span id="source-8"></span> [OpenAI · Broadening access to AI for scientists](https://openai.com/index/expanding-chatgpt-for-academic-researchers)

[9] <span id="source-9"></span> [GitHub · Bulk start AI Scan for repositories](https://github.blog/changelog/2026-11-16-bulk-start-ai-scan-for-repositories)

[10] <span id="source-10"></span> [OpenAI · LVMH’s Media Division announces strategic partnership with OpenAI](https://openai.com/index/lvmh-media-division-partnership)
