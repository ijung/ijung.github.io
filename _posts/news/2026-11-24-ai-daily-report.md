---
title: "AI 데일리 리포트 — 2026-11-24"
date: "2026-11-24 06:00:39 +0900"
categories: ["뉴스", "AI"]
tags: ["AI", "데일리-리포트"]
toc: true
comments: false
permalink: /posts/ai-daily-report-2026-11-24/
---

검수 기준: 2026-11-24 06:09 KST

## 30초 브리핑

- Z.ai가 시각 입력을 지원하는 GLM-5.4를 발표하고 Coding Plan 전 등급에 제공합니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>
- AWS가 DevOps Agent의 코드 실행 환경을 사용자 정의 에이전트와 모든 상용 리전으로 확대했습니다.<a href="#source-3">[3]</a>
- AWS의 자동 조사 품질평가는 미리보기 기간에 대상 조사의 최대 10%만 자동 평가하도록 제한됩니다.<a href="#source-3">[3]</a>

## 오늘의 주요 AI 뉴스

### GLM-5.4 출시, 같은 모델 층을 반복 사용하는 코딩 모델

**Z.ai는 11월 23일 GLM-5.4를 발표했으며, 공식 이용 안내는 Lite·Pro·Max 구독자 모두의 모델 전환을 지원한다고 명시합니다.**<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

<p id="def-loop">루프형 트랜스포머는 같은 가중치를 공유하는 모델 층을 여러 번 통과해 내부 표현을 다듬는 구조입니다.<a href="#source-1">[1]</a> GLM-5.4는 기존 GLM-5의 28개 층을 두 차례 실행하며, 회사는 이 방식 자체가 업계 최초라고 주장하지 않습니다.<a href="#source-1">[1]</a></p>

- **달라진 점:** 시각 입력을 활용한 코딩과 함께 영상·게임·3D 모델 제작 사례를 제시했습니다.<a href="#source-1">[1]</a> <span id="def-generalized">이번 발표의 Generalized Coding은 코드를 소프트웨어 개발 밖의 영상·설계 등 더 넓은 작업을 수행하는 수단으로 쓰겠다는 회사의 개념입니다.<a href="#source-1">[1]</a></span>
- **왜 중요한지:** 모델 크기를 늘리는 것과 별개로 계산을 반복하는 구조를 실제 구독 서비스에 적용했다는 의미가 있습니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a> 다만 회사는 지나치게 긴 추론에 빠지는 한계를 인정했으며, 성능 우위와 실제 작업 완료 시간은 이번 조사에서 독립 검증하지 않았습니다.<a href="#source-1">[1]</a>

<p id="def-api">API는 개발한 앱에서 모델을 호출하는 연결 창구입니다.<a href="#source-2">[2]</a> 구독용 코딩 도구 연결은 제공되지만, 이번 발표에서 일반 API는 아직 예정이며 내려받아 실행할 모델 가중치도 안전성 보강 후 공개할 계획입니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a></p>

<p class="news-source"><a href="https://z.ai/blog/glm-5.4">Z.ai · GLM-5.4: Toward Generalized Coding with Our First Loop Transformer</a><br><span class="source-published">게시: 2026-11-23 · 시각 미공개</span></p>

<p class="news-source"><a href="https://docs.z.ai/devpack/latest-model">Z.ai · How to Switch Models</a><br><span class="source-published">게시: 확인 필요 · 현행 이용 안내</span><br><span class="source-modified">수정: 2026-11-23 19:17 KST</span></p>

### AWS DevOps Agent, 사용자 정의 에이전트에도 샌드박스 제공

**AWS의 11월 21일 변경 기록은 코드 실행 환경을 사용자 정의 에이전트로 확대하고, 모든 상용 AWS 리전에서 제공한다고 안내합니다.**<a href="#source-3">[3]</a>

<p id="def-sandbox">샌드박스는 에이전트가 코드를 작성·실행하는 격리된 임시 Linux 환경입니다.<a href="#source-4">[4]</a> 이번 기능은 개별 장애 조사·대화·에이전트 실행마다 환경을 따로 만들며, 현재 미리보기입니다.<a href="#source-4">[4]</a></p>

- **달라진 점:** 장애 조사와 채팅뿐 아니라 사용자 정의 에이전트도 코드를 실행할 수 있습니다.<a href="#source-3">[3]</a> 필요한 프로그램 패키지를 어디서 설치할지 선택할 수 있지만, AWS 관리형 패키지 저장소는 일부 리전에서만 제공됩니다.<a href="#source-3">[3]</a><a href="#source-4">[4]</a>
- **왜 중요한지:** 답변 생성에 더해 로그 분석·통계 계산을 코드로 수행할 범위가 넓어졌습니다.<a href="#source-4">[4]</a> 다만 샌드박스의 AWS 호출은 읽기 전용으로 제한되며, 외부 통신은 허용된 주소·요청 방식·경로와 설정한 패키지 저장소의 범위를 따릅니다.<a href="#source-4">[4]</a> 코드 실행 가능성이 AWS 자원 변경 권한까지 뜻하지는 않습니다.<a href="#source-4">[4]</a>

11월 21일 변경을 이번에 뒤늦게 보완한 소식이며, 오늘 처음 출시된 기능으로 표시하지 않았습니다.

<p class="news-source"><a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/whats-new.html">AWS · What&#x27;s new — DevOps Agent</a><br><span class="source-published">게시: 2026-11-21 · 시각 미공개 · 변경 기록의 해당 항목</span></p>

<p class="news-source"><a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/configuring-integrations-and-knowledge-sandbox.html">AWS · Set up a Sandbox</a><br><span class="source-published">게시: 확인 필요 · 현행 안내 · 변경 기록: 2026-11-21 · 시각 미공개</span></p>

## 기타 뉴스

<ul>
<li><strong>AWS 자동 조사 품질평가, 미리보기 표본 비율 상한 명시</strong><br>11월 22일 변경 기록에 따르면 완료된 장애 조사 결과를 자동 평가하는 Reviews는 미리보기 동안 대상 조사의 1~10%를 정수 비율로 설정하며, 상한은 10%입니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a> 이는 기능의 최초 출시가 아니라 평가 범위에 대한 후속 안내입니다.<a href="#source-3">[3]</a>
<p class="news-source"><a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/whats-new.html">AWS · What&#x27;s new — DevOps Agent</a><br><span class="source-published">게시: 2026-11-22 · 시각 미공개 · 변경 기록의 해당 항목</span></p>
<p class="news-source"><a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/working-with-devops-agent-reviewing-investigations.html">AWS · Reviewing investigations</a><br><span class="source-published">게시: 확인 필요 · 현행 안내 · 변경 기록: 2026-11-22 · 시각 미공개</span></p>
</li>
</ul>

## AI 트렌드와 변화 신호

- **실행 확대와 통제 조건의 동시 구체화:** 이번에 확인한 변화는 코딩 모델의 반복 계산 구조, 에이전트의 격리 실행, 조사 결과의 표본 평가입니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-4">[4]</a> 기능이 있다는 사실과 어느 범위까지 사용할 수 있는지는 분리해서 읽어야 합니다.
- **전날 대비:** 전날 보고서와 사건을 대조해 유출 공지·사내망 연결·평가 지표 제안은 반복하지 않고, 이번에는 새 모델과 운영 에이전트의 실행·평가 조건을 선별했습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a> 시장 전체 관심도의 상승·하락을 측정한 결과는 아닙니다.

## 조사 한계

- 수집 마감은 2026-11-24 06:00:39 KST입니다. 최근 24시간을 우선하고 최대 72시간 안의 미보도 중요 변경을 보완했습니다. 날짜만 공개된 자료는 발표 시각을 추정하지 않았습니다.
- 네 공식 목록을 직접 읽었지만 첫 화면 범위·월 단위 날짜·상단과 개별 항목의 날짜 차이로 전체 기간의 누락 여부는 확정하지 못했습니다. 추가 정책·투자 후보는 기간이나 최신 원문 근거가 부족해 제외했습니다.
- Z.ai 발표문은 본문 추출로 읽었으나 별도 직접 HTTP 확인에서는 본문이 비어 있었습니다. 이용 안내와 AWS 자료는 직접 본문도 대조했습니다. 현행 안내 문서의 최초 게시일은 확인하지 못해 날짜가 있는 발표·변경 기록에 연결했습니다. 회사의 성능·안전 효과와 계정별 실제 제공 상태는 직접 시험하지 않았으며, 필요한 용어는 본문에서 설명했습니다.

## Sources

<p id="source-1" markdown="0">[1] <a href="https://z.ai/blog/glm-5.4">Z.ai · GLM-5.4: Toward Generalized Coding with Our First Loop Transformer</a></p>

<p id="source-2" markdown="0">[2] <a href="https://docs.z.ai/devpack/latest-model">Z.ai · How to Switch Models</a></p>

<p id="source-3" markdown="0">[3] <a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/whats-new.html">AWS · What&#x27;s new — DevOps Agent</a></p>

<p id="source-4" markdown="0">[4] <a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/configuring-integrations-and-knowledge-sandbox.html">AWS · Set up a Sandbox</a></p>

<p id="source-5" markdown="0">[5] <a href="https://docs.aws.amazon.com/devopsagent/latest/userguide/working-with-devops-agent-reviewing-investigations.html">AWS · Reviewing investigations</a></p>
