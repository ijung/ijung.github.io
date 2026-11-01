---
title: "AI 데일리 리포트 — 2026-11-02"
date: "2026-11-02 06:07:09 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-02/
---

검수 기준: 2026-11-02 06:07 KST

## 30초 브리핑

- OpenAI가 12월부터 Plus·Pro·Business의 ChatGPT와 Codex 사용 한도를 통합할 계획을 안내했습니다.<a href="#source-1">[1]</a> Instant 채팅은 차감 대상에서 제외됩니다.<a href="#source-1">[1]</a>
- Microsoft 공식 일정상 AI Builder 용량 부가상품의 판매 종료일인 11월 1일이 도래했습니다.<a href="#source-2">[2]</a> 기존 구독의 크레딧은 현재 계약 종료까지 사용할 수 있습니다.<a href="#source-2">[2]</a>
- Cursor가 변조된 Claude Code 확장 프로그램의 영향 버전과 식별 정보를 보완했습니다.<a href="#source-3">[3]</a> 확장 프로그램 삭제만으로 해결되지 않을 수 있다는 점이 중요합니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### ChatGPT·Codex, 12월부터 하나의 사용 한도 공유

**OpenAI는 Plus·Pro·Business에서 ChatGPT와 Codex의 사용량을 함께 계산할 예정이라고 안내했습니다.**<a href="#source-1">[1]</a>

<p id="def-token">토큰은 모델이 처리하고 생성하는 정보의 단위입니다.<a href="#source-1">[1]</a> 통합 한도는 메시지 개수가 아니라 토큰을 기준으로 측정하며, 선택한 모델과 작업에 따라 차감량이 달라집니다.<a href="#source-1">[1]</a></p>

- **달라지는 점:** ChatGPT Work의 작업 수행 기능을 주 ChatGPT 경험에 통합하면서 사용 한도도 공유합니다.<a href="#source-1">[1]</a> Instant 채팅은 이 한도를 차감하지 않습니다.<a href="#source-1">[1]</a>
- **왜 중요한지:** 대화와 코딩을 별도 이용량으로 생각하던 사용자는 앞으로 두 작업의 합산 사용량을 보게 됩니다.<a href="#source-1">[1]</a> 다만 정확한 시행일과 요금제별 토큰 한도는 공개되지 않아, 이용량이 실제로 늘거나 줄어드는지는 아직 판단할 수 없습니다.<a href="#source-4">[4]</a><a href="#source-1">[1]</a>

<p class="news-source"><a href="https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits?ref=explainx">OpenAI · Understanding ChatGPT and Codex usage limits</a><br><span class="source-published">게시: 확인 필요 · 원문에 상대 갱신 시각만 표시</span></p>

<p class="news-source"><a href="https://explainx.ai/blog/chatgpt-codex-usage-limits-merge-shared-token-allowance-december-2026">explainx.ai · ChatGPT + Codex Limits Merge in December</a><br><span class="source-published">게시: 2026-11-01 09:00 KST</span></p>

### AI Builder 부가상품 판매 종료일 도래 — 기존 크레딧은 계약 종료까지

**Microsoft의 공식 안내상 11월 1일부터 AI Builder 용량 부가상품 판매가 종료됩니다.<a href="#source-2">[2]</a> 기존 구독의 즉시 사용 중단과는 다릅니다.**<a href="#source-2">[2]</a>

- **달라진 점:** 10월 22일 공지한 판매 종료 일정이 도래했습니다.<a href="#source-2">[2]</a> 영향 대상은 해당 부가상품을 이용하는 고객을 둔 Microsoft 클라우드 판매 파트너입니다.<a href="#source-2">[2]</a>
- **왜 중요한지:** 유효한 구독의 AI Builder 크레딧은 현재 계약 기간이 끝날 때까지 사용할 수 있습니다.<a href="#source-2">[2]</a> Microsoft는 다음 갱신 전 자동 갱신 해제와, 기존 크레딧 만료 이후 Copilot Credits 사용 계획을 안내합니다.<a href="#source-2">[2]</a> 판매 종료와 기존 사용 권한의 만료를 구분해야 한다는 의미가 있습니다.<a href="#source-2">[2]</a>

<p class="news-source"><a href="https://learn.microsoft.com/en-us/partner-center/announcements/2026-october">Microsoft · October 2026 announcements</a><br><span class="source-published">게시: 2026-10-22 · 시각 미공개</span></p>

### Cursor 보안 공지 보완 — 변조 확장 프로그램의 영향 범위 구체화

**Cursor는 원문 현지 시각 10월 30일 보안 공지를 수정해 영향 버전과 침해 식별 정보를 명확히 했습니다.<a href="#source-3">[3]</a> 새 공격 발생 발표가 아니라 기존 사건의 후속 안내입니다.**<a href="#source-3">[3]</a>

- **확인된 범위:** 공지는 10월 29일 특정 시간대에 Cursor 확장 프로그램 마켓플레이스에서 배포된 macOS·Windows용 Anthropic.claude-code 2.1.312·2.1.313을 대상으로 합니다.<a href="#source-3">[3]</a> Cursor는 이 변조본을 Anthropic이 개발한 것이 아니라고 설명합니다.<a href="#source-3">[3]</a>
- **왜 중요한지:** 공격 서버가 차단됐고 원격 접속 시도가 일반적인 환경에서 실패하더라도, 무단 접속 키와 계속 실행되는 악성 파일이 남을 수 있습니다.<a href="#source-3">[3]</a> Cursor는 확장 프로그램 제거만으로 충분하지 않을 수 있다고 경고합니다.<a href="#source-3">[3]</a> The Register는 Anthropic 측의 공식 배포 채널은 영향받지 않았다는 설명을 별도로 취재했습니다.<a href="#source-5">[5]</a>

<p class="news-source"><a href="https://cursor.com/blog/claude-code-extension-security-notice">Cursor · Claude Code 확장 프로그램 보안 공지</a><br><span class="source-published">게시: 2026-10-29 21:00 KST</span><br><span class="source-modified">수정: 2026-10-31 03:30 KST</span></p>

<p class="news-source"><a href="https://www.theregister.com/cyber-crime/2026/10/30/anthropic-that-claude-code-extension-compromise-couldnt-be-us/5306682">The Register · Anthropic: That Claude Code extension compromise… couldn’t be us</a><br><span class="source-published">게시: 2026-10-31 01:27 KST</span></p>

## AI 트렌드와 변화 신호

- **관찰 키워드 — 사용 한도·계약 수명:** ChatGPT·Codex의 한도 통합 예고와 AI Builder 판매 종료 일정은 기능뿐 아니라 사용량 계산 방식과 기존 이용 권한의 종료 조건도 살펴야 한다는 점을 보여줍니다.<a href="#source-1">[1]</a> 이는 이번 두 사례에 대한 해석이며 업계 전체의 가격 인상을 뜻하지는 않습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- **전날 대비:** 전날 보고서의 모델 출시·평가 도구 전환 소식과 다른 사건을 선별했습니다. 오늘의 초점은 사용 한도 변경 예고, 판매 종료 일정 도래, 개발 도구 배포 경로의 보안 후속 정보입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a>

## 조사 한계

- 최근 24시간의 모델·제품, 에이전트·개발 도구, 규제·정책, 투자·파트너십을 우선 조사했습니다. Cursor는 최근 72시간 안의 공지 보완을 배경으로 포함했으며, 최초 사건은 그보다 앞선 10월 29일입니다.<a href="#source-3">[3]</a>
- OpenAI 도움말 본문은 확인했으나 최초 게시일은 확인하지 못했습니다. 원문의 상대 갱신 표시와 11월 1일 설명 기사를 최신성 보조 근거로 사용했으며, 게시일을 갱신 시각으로 대체하지 않았습니다.<a href="#source-4">[4]</a><a href="#source-1">[1]</a> Microsoft 항목은 공식 일정의 도래를 확인한 것으로, 고객별 실제 판매 화면이나 계약 상태를 시험한 결과는 아닙니다.<a href="#source-2">[2]</a>
- 기업 공지는 발표·적용 조건의 근거입니다. 보안 사건의 전체 피해 규모와 침입 경로는 확정하지 않았으며, 이번 조사에서 추가 검증하지 못한 모델 성능이나 미확정 거래는 포함하지 않았습니다.

## Sources

[1] <span id="source-1"></span> [OpenAI · Understanding ChatGPT and Codex usage limits](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits?ref=explainx)

[2] <span id="source-2"></span> [Microsoft · October 2026 announcements](https://learn.microsoft.com/en-us/partner-center/announcements/2026-october)

[3] <span id="source-3"></span> [Cursor · Claude Code 확장 프로그램 보안 공지](https://cursor.com/blog/claude-code-extension-security-notice)

[4] <span id="source-4"></span> [explainx.ai · ChatGPT + Codex Limits Merge in December](https://explainx.ai/blog/chatgpt-codex-usage-limits-merge-shared-token-allowance-december-2026)

[5] <span id="source-5"></span> [The Register · Anthropic: That Claude Code extension compromise… couldn’t be us](https://www.theregister.com/cyber-crime/2026/10/30/anthropic-that-claude-code-extension-compromise-couldnt-be-us/5306682)
