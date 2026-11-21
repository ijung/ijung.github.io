---
title: "AI 데일리 리포트 — 2026-11-22"
date: "2026-11-22 06:00:23 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-22/
---

검수 기준: 2026-11-22 06:08 KST

## 30초 브리핑

- Thinking Machines Lab이 로컬 실행용 Inkling 2 Mini의 모델 가중치를 공개했습니다.<a href="#source-1">[1]</a>
- Google이 미국의 유료 개인 계정부터 Gemini의 Mac 앱 조작 기능을 순차 제공합니다.<a href="#source-2">[2]</a>
- OpenAI가 GPT-Live-1.1의 개발자 제공과 무음 시 추론을 멈추는 대기 설정을 추가했습니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### Inkling 2 Mini 공개, 로컬 실행을 겨냥한 반복 계산 구조

**Thinking Machines Lab이 11월 20일 텍스트·이미지를 입력받아 텍스트로 답하는 Inkling 2 Mini를 공개했습니다.**<a href="#source-1">[1]</a>

<p id="def-open-weights">오픈 웨이트는 개발자가 내려받아 실행·조정할 수 있도록 모델의 학습된 가중치를 공개하는 방식입니다. 이번 모델은 Apache 2.0 라이선스로 배포됩니다.<a href="#source-1">[1]</a></p>

<p id="def-looped">루프형 트랜스포머는 같은 층의 가중치를 재사용해 계산을 반복하는 모델 구조입니다. Inkling 2 Mini는 전체 24개 층 중 가운데 12개 층을 세 번 통과합니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a></p>

- **달라진 점:** 회사는 메모리 32GB 이상인 Apple silicon 기기와 NVIDIA DGX Spark의 로컬 실행을 지원한다고 안내했습니다.<a href="#source-1">[1]</a> Hugging Face도 관련 실행 도구 지원을 공개했습니다.<a href="#source-4">[4]</a>
- **왜 중요한지:** 모델을 더 크게 저장하는 대신 같은 가중치로 계산을 반복하는 선택지를 로컬 개발자에게 제공한다는 의미가 있습니다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a> 다만 회사는 실행 속도가 하드웨어·실행 도구에 따라 달라지고, 도구 사용 중 확인 없이 파일을 삭제·덮어쓸 수 있다고 경고했습니다.<a href="#source-1">[1]</a>

<p class="news-source"><a href="https://huggingface.co/thinkingmachines/Inkling-2-Mini">Thinking Machines Lab · Inkling 2 Mini 모델 카드</a><br><span class="source-published">게시: 확인 필요 · 공식 출시: 2026-11-20 · 시각 미공개</span></p>

<p class="news-source"><a href="https://huggingface.co/blog/inkling2mini">Hugging Face · Welcome Inkling 2 Mini, Cutting-edge Performance for your Local Setup</a><br><span class="source-published">게시: 2026-11-20 09:00 KST</span></p>

### Gemini의 Mac 앱 조작, 미국 개인 계정부터 제공

**Google은 11월 19일 Mac의 파일·앱·웹을 넘나드는 작업을 Gemini에 맡기는 기능의 순차 제공을 발표했습니다.**<a href="#source-2">[2]</a>

<p id="def-computer-use">컴퓨터 사용(computer use)은 AI가 화면의 메뉴를 탐색하고 클릭·입력·스크롤해 앱을 조작하는 기능입니다. 이번 기능은 별도의 가상 화면에서 진행되며, 사용자는 채팅의 실시간 미리보기를 보면서 다른 작업을 계속할 수 있습니다.<a href="#source-2">[2]</a><a href="#source-5">[5]</a></p>

- **달라진 점:** 문서에서 정보를 추출한 뒤 다른 앱의 양식에 입력하는 등 여러 단계의 작업을 수행합니다.<a href="#source-2">[2]</a> 미국·영어 환경의 18세 이상 개인 계정과 Google AI Plus·Pro·Ultra가 대상이며, Apple silicon Mac과 macOS Sequoia 15.0 이상이 필요합니다.<a href="#source-2">[2]</a> 회사·학교 계정은 현재 제외됩니다.<a href="#source-5">[5]</a>
- **왜 중요한지:** 답변 생성에서 실제 앱 조작으로 작업 범위가 넓어집니다.<a href="#source-2">[2]</a> 다만 Google 도움말은 허용된 앱의 로그인 세션 접근, 화면 속 불신 콘텐츠에 의한 다른 앱 조작 유도, 인증 코드 접근 가능성을 명시합니다.<a href="#source-5">[5]</a> 별도 작업 화면이 이런 위험까지 차단한다는 뜻은 아닙니다.<a href="#source-5">[5]</a>

한국 제공은 이번 발표 범위에 포함되지 않습니다.<a href="#source-2">[2]</a><a href="#source-5">[5]</a>

<p class="news-source"><a href="https://blog.google/innovation-and-ai/products/gemini-app/computer-use-mac">Google · Let Gemini tackle repetitive chores on your Mac</a><br><span class="source-published">게시: 2026-11-20 02:00 KST</span><br><span class="source-modified">수정: 2026-11-20 02:09 KST</span></p>

<p class="news-source"><a href="https://support.google.com/gemini/answer/18956635?hl=en-GB">Google 도움말 · Use Gemini to work with apps on your Mac</a><br><span class="source-published">게시: 확인 필요 · 현행 도움말</span></p>

### GPT-Live-1.1, 음성 앱 개발에 제공하고 무음 대기 설정 추가

**OpenAI의 11월 19일 개발자 변경 기록에 GPT-Live-1.1과 무음 대기 설정이 추가됐습니다.**<a href="#source-3">[3]</a>

<p id="def-api">API는 개발한 앱이 모델을 호출해 기능을 사용하는 연결 창구입니다. 이번 변경은 GPT-Live-1.1을 기존 음성 세션 설정에서 선택할 수 있게 하는 개발자 제공입니다.<a href="#source-3">[3]</a></p>

<p id="def-standby">이번 대기 모드(standby)는 음성 통화의 무음 구간에서 추론을 잠시 멈추고 사용자가 다시 말하면 재개하는 설정입니다. GPT-Live-1과 GPT-Live-1.1의 WebRTC 음성 세션에 적용됩니다.<a href="#source-3">[3]</a></p>

- **달라진 점:** OpenAI는 다국어 대화의 발음·억양 일관성·음성 받아쓰기를 개선했다고 안내했습니다.<a href="#source-3">[3]</a> 이는 회사 발표이며, 이번 조사에서 실제 대화 품질을 시험하지 않았습니다.
- **왜 중요한지:** 음성 모델 선택뿐 아니라 사용자가 말하지 않는 구간의 처리 방식도 조정할 수 있다는 변화입니다.<a href="#source-3">[3]</a> 이 보고서에서는 가격이나 전체 운영비 절감률을 확정하지 않았습니다.

<p class="news-source"><a href="https://developers.openai.com/api/docs/changelog">OpenAI · API Changelog</a><br><span class="source-published">게시: 2026-11-19 · 시각 미공개 · 변경 기록의 해당 항목</span></p>

## AI 트렌드와 변화 신호

- **작업 위치와 통제 범위가 중요합니다:** 로컬 모델의 파일 변경 경고와 데스크톱 에이전트의 로그인 세션 접근은 실행 가능성과 안전한 위임을 별도로 검토해야 한다는 신호입니다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a>
- **전날 대비:** 전날의 연구용 평가·기업공개 후속 소식을 반복하지 않고, 이번에는 미보도 상태였던 로컬 모델·Mac 앱 조작·개발자용 음성 기능을 보완했습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a> 시장 전체의 관심도 상승·하락을 측정한 결과는 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-22 06:00:23 KST입니다. 최근 24시간을 우선하고 최대 72시간 안의 미보도 발표를 확인했습니다. 위 소식은 11월 19~20일 발표를 뒤늦게 보완한 내용이며, 오늘 새로 발표된 사건으로 표시하지 않았습니다.
- 네 공식 목록을 직접 읽었지만 일부 목록의 월 단위 날짜·상단과 개별 항목의 날짜 차이·첫 화면 범위 때문에 전체 기간의 누락 여부는 확정하지 못했습니다. 추가 투자·정책 후보는 최신 원문 근거가 부족해 제외했습니다.
- 모델 가중치 페이지와 Google 도움말은 최초 게시일을 확인하지 못했습니다. 각각 공식 출시일과 날짜가 확인된 발표문으로 최신성을 연결했으며, OpenAI 변경 기록은 해당 항목의 날짜만 확인했습니다. 실제 성능·계정별 제공은 직접 시험하지 않았고, 필요한 용어는 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://huggingface.co/thinkingmachines/Inkling-2-Mini">Thinking Machines Lab · Inkling 2 Mini 모델 카드</a></p>

<p id="source-2" markdown="0">[2] <a href="https://blog.google/innovation-and-ai/products/gemini-app/computer-use-mac">Google · Let Gemini tackle repetitive chores on your Mac</a></p>

<p id="source-3" markdown="0">[3] <a href="https://developers.openai.com/api/docs/changelog">OpenAI · API Changelog</a></p>

<p id="source-4" markdown="0">[4] <a href="https://huggingface.co/blog/inkling2mini">Hugging Face · Welcome Inkling 2 Mini, Cutting-edge Performance for your Local Setup</a></p>

<p id="source-5" markdown="0">[5] <a href="https://support.google.com/gemini/answer/18956635?hl=en-GB">Google 도움말 · Use Gemini to work with apps on your Mac</a></p>
