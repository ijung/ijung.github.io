---
title: "Herdr: 여러 코딩 에이전트의 터미널을 한곳에서 관리하기"
date: 2026-10-26 11:06:26 +0900
categories: [개발, 개발도구]
tags: [herdr, ai-agent, terminal, developer-tools]
toc: true
comments: false
permalink: /posts/herdr-coding-agent-runtime/
---

코딩 에이전트를 여러 개 켜 두면 코드를 작성하는 일 말고도 관리할 일이 생긴다. 어느 터미널에서 테스트가 돌고 있는지, 어떤 에이전트가 답변을 기다리는지, 원격 서버에서 시작한 작업은 어떻게 돌아가고 있는지 확인해야 한다.

**Herdr는 이런 상황을 위한 터미널 기반 실행·관리 도구다.** 새로운 코딩 모델로 기존 에이전트를 대체하기보다, Claude Code·Codex·OpenCode·Hermes Agent 같은 CLI가 실행되는 터미널을 관리하고 상태를 모아 보여주는 방향을 택한다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>

> 이 글은 2026년 10월 26일 확인한 공식 사이트·문서·저장소 README를 바탕으로 작성한 프로젝트 소개다. 직접 설치하거나 성능·복구 동작을 실험한 사용 후기는 아니다. 문서에 적힌 기능과 실제 운영에서 검증해야 할 부분을 구분한다.

## 어떤 문제를 해결하나

Herdr의 기본 단위는 작업 공간, 탭, pane이다. 에이전트는 실제 터미널 pane 안에서 실행되며 셸·로그·질문·프로세스가 그 안에 남는다. Herdr는 에이전트의 상태를 작업 공간과 탭까지 모아 보여줘, 사용자가 모든 터미널을 일일이 순회하지 않고 주의가 필요한 작업을 찾도록 돕는다.<a href="#source-3">[3]</a>

예를 들어 한 프로젝트에서는 구현을 맡기고, 다른 프로젝트에서는 테스트나 문서 작업을 진행하는 상황을 생각할 수 있다. 이때 관심사는 단순히 창을 나누는 것이 아니라 **어느 작업이 진행 중이고, 어디에 사람의 판단이 필요한가**다. Herdr의 상태 표시와 작업 공간 구성을 이런 관리 문제에 활용할 수 있다는 것이 이 글의 해석이다.<a href="#source-3">[3]</a>

<figure>
  <img src="/assets/img/posts/herdr-coding-agent-runtime/herdr-runtime-overview.png" alt="Herdr 클라이언트와 로컬 서버, 터미널 pane 및 별도 원격 머신의 관계를 보여주는 설명용 구조도" width="800" height="1060" loading="lazy" style="max-width:100%;height:auto;">
  <figcaption>그림 1. 클라이언트는 화면과 입력을 제공하고, 각 머신의 Herdr 서버는 자체 pane과 프로세스를 관리한다. 원격 머신은 SSH로 연결한다. 공식 문서를 바탕으로 직접 제작한 설명용 개념도이며 실제 제품 화면이 아니다. <a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-5">[5]</a> <a href="/assets/img/posts/herdr-coding-agent-runtime/herdr-runtime-overview.png">원본 이미지 보기</a></figcaption>
</figure>

## 핵심 기능 1: 화면을 닫아도 서버 쪽 작업은 유지

Herdr는 클라이언트와 서버를 구분한다. 정상적으로 클라이언트를 detach해도 Herdr 서버가 계속 실행 중이면 pane 안의 셸, 에이전트, 테스트 등 프로세스가 유지된다. 공식 문서는 detach 후 다시 접속하는 경로를 원래 프로세스가 멈추지 않는 가장 강한 지속 방식으로 설명한다.<a href="#source-2">[2]</a>

단, **접속 종료와 서버·머신 재시작은 같은 상황이 아니다.** 서버가 종료되면 기존 pane 프로세스도 사라진다. 다시 시작할 때 저장된 작업 공간·탭·디렉터리·레이아웃을 복원하는 것이지, 실행 중이던 임의의 테스트나 서버 프로세스를 그 순간 그대로 되살리는 기능은 아니다.<a href="#source-2">[2]</a>

지원되는 에이전트는 유효한 네이티브 세션 정보와 integration을 통해 대화를 재개할 수 있다. 문서상 이 재개는 클라이언트가 접속해 터미널 크기와 테마 정보를 전달한 뒤 진행된다. 따라서 “컴퓨터를 재부팅해도 모든 작업이 무인으로 이어진다”는 식으로 이해하면 안 된다.<a href="#source-2">[2]</a>

원격 머신의 Herdr 서버가 살아 있다면 접속한 노트북의 네트워크가 끊어져도 원격 작업을 유지할 수 있다. 반면 작업이 실행되는 머신 자체의 종료까지 같은 방식으로 견딘다고 볼 수는 없다.<a href="#source-2">[2]</a><a href="#source-5">[5]</a>

<figure>
  <img src="/assets/img/posts/herdr-coding-agent-runtime/herdr-persistence.png" alt="클라이언트 접속 종료 시 프로세스 유지와 서버 재시작 뒤 레이아웃 및 지원 대화 복원의 차이" width="800" height="1060" loading="lazy" style="max-width:100%;height:auto;">
  <figcaption>그림 2. 접속만 끊으면 살아 있는 서버의 프로세스는 유지되지만, 서버가 종료되면 기존 프로세스는 사라진다. 레이아웃 복원과 지원되는 대화 재개는 실행 상태 보존과 다르다. 설명용 개념도. <a href="#source-2">[2]</a> <a href="/assets/img/posts/herdr-coding-agent-runtime/herdr-persistence.png">원본 이미지 보기</a></figcaption>
</figure>

## 핵심 기능 2: 일하는 에이전트와 기다리는 에이전트 구분

Herdr는 `working`, `blocked`, `idle` 같은 상태를 표시한다. 지원 방식에 따라 에이전트 프로세스와 터미널 하단 화면을 감지 규칙으로 해석하거나, integration·에이전트가 직접 보고한 상태를 이용한다.<a href="#source-3">[3]</a>

여기에는 중요한 한계가 있다. 새로운 형태의 승인 질문을 규칙이 알아보지 못하면 실제로는 입력을 기다리는데도 `idle`로 표시될 수 있다. 공식 문서도 **idle 상태는 어떤 행위를 승인해도 된다는 뜻이 아니다**라고 경고한다.<a href="#source-3">[3]</a>

또한 일부 에이전트는 백그라운드 작업이나 하위 에이전트가 남아 있어도 프롬프트로 돌아오면 idle로 판단된다. 상태 대기는 에이전트의 의미상 상태를 관찰하는 기능이지, 모든 테스트와 백그라운드 작업의 완료 증명은 아니다.<a href="#source-3">[3]</a><a href="#source-4">[4]</a>

실무에서는 상태 표시를 “어디를 먼저 확인할까”를 정하는 신호로 쓰고, 완료 판단은 테스트 결과·산출물·실제 서비스 상태로 따로 확인하는 편이 적절하다.

<figure>
  <img src="/assets/img/posts/herdr-coding-agent-runtime/herdr-agent-states.png" alt="working, blocked, idle 상태의 의미와 완료 및 승인을 별도로 확인해야 한다는 주의사항" width="800" height="1060" loading="lazy" style="max-width:100%;height:auto;">
  <figcaption>그림 3. 상태 표시는 확인할 작업을 찾는 신호다. idle을 승인이나 모든 작업의 완료로 간주하지 말고 질문·테스트·산출물을 확인해야 한다. 설명용 개념도. <a href="#source-3">[3]</a><a href="#source-4">[4]</a> <a href="/assets/img/posts/herdr-coding-agent-runtime/herdr-agent-states.png">원본 이미지 보기</a></figcaption>
</figure>

## 핵심 기능 3: CLI와 소켓 API로 자동화

Herdr는 작업 공간 생성, pane 분할과 읽기, 에이전트 시작·프롬프트 전달·상태 대기 등을 CLI와 로컬 소켓 API로 제공한다. 공식 문서는 일반적인 자동화는 CLI wrapper부터 시작하고, 직접적인 요청·응답 제어나 장기 이벤트 구독이 필요한 경우 raw API를 사용하도록 안내한다.<a href="#source-4">[4]</a>

이 기능을 조합하면 여러 에이전트의 터미널 작업을 스크립트로 연결하는 흐름을 설계할 수 있다. 다만 **명령 입력을 접수했다는 응답과 명령 실행이 끝났다는 증거는 다르다.** 문서에서도 입력 API의 성공이 큐에 들어갔다는 의미일 수 있음을 명시한다.<a href="#source-4">[4]</a>

소켓 API는 단순 상태 조회 장치가 아니다. pane에 입력을 보내거나 서버를 중지하는 제어 기능도 포함한다. 따라서 접근 권한을 가진 프로세스와 사용자, 공유하는 세션 경로를 신중히 관리해야 한다. 이는 제공된 제어 기능을 바탕으로 한 운영상 권고이며, 이 글에서 별도의 보안 감사를 수행한 것은 아니다.<a href="#source-4">[4]</a>

## 핵심 기능 4: 여러 머신의 작업 공간을 한 화면에

SSH로 저장한 원격 머신을 연결하면 로컬 작업 공간과 원격 작업 공간을 같은 클라이언트에서 전환할 수 있다. 각 머신은 자체 Herdr 서버·세션·프로세스를 유지하며, 한 머신과의 연결이 끊어졌다고 다른 머신의 연결까지 종료되는 구조는 아니다.<a href="#source-5">[5]</a>

연결이 끊긴 뒤 흐리게 남는 상태 표시는 마지막으로 받은 캐시이며 실시간 상태가 아니다. 또 원격 설정 과정은 사용자 확인 뒤 설치·업데이트·서버 교체를 수행할 수 있지만, 백그라운드 재접속은 이런 변경을 자동으로 실행하지 않는다고 문서는 구분한다.<a href="#source-5">[5]</a>

원격 대상을 다루는 자동화에서는 UI에서 선택한 머신과 CLI의 실제 대상도 구분해야 한다. 기존 pane에서 실행한 CLI는 그 pane이 상속한 세션·소켓을 사용하므로, 원격 제어에는 명시적인 머신 지정이 필요하다.<a href="#source-5">[5]</a>

## 도입 전에 확인할 것

- **내 에이전트의 지원 수준:** “터미널에서 실행 가능”, “상태 감지 가능”, “재시작 후 대화 재개 가능”은 서로 다른 조건이다. integration 요구사항을 확인한다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>
- **실행 머신과 복구 조건:** detach, 네트워크 단절, 서버 재시작을 나눠 시험한다. 레이아웃 복원과 프로세스 지속을 혼동하지 않는다.<a href="#source-2">[2]</a>
- **화면 기록의 민감정보:** pane history는 프롬프트·토큰·명령 출력 등을 담을 수 있어 기본 비활성화다. 활성화 여부와 저장 경로를 검토한다.<a href="#source-2">[2]</a>
- **기존 tmux 구성:** Herdr pane 안에서 다시 실행한 tmux의 내부 에이전트는 감지하지 못한다. 기존 셸 자동 attach 설정과 함께 확인한다.<a href="#source-3">[3]</a>
- **원격 접근 범위:** SSH 호스트 키 확인과 실행 대상 확인을 유지하고, 연결 캐시를 현재 상태로 오인하지 않는다.<a href="#source-5">[5]</a>
- **라이선스와 비용 범위:** 공식 README는 Herdr를 Apache License 2.0으로 표기한다. 이는 각 에이전트 서비스의 사용료나 플러그인의 개별 조건까지 확인했다는 뜻은 아니다.<a href="#source-6">[6]</a>

프로젝트 사이트는 macOS·Linux·Windows를 지원 대상으로 소개한다. 실제 도입 시에는 사용하는 OS와 필요한 기능의 세부 지원 여부를 설치 문서에서 다시 확인하는 것이 좋다.<a href="#source-1">[1]</a>

## 누구에게 잘 맞을까

공식 기능을 기준으로 보면, 여러 코딩 CLI를 병행하거나 원격 머신에서 작업을 이어가는 개발자에게 검토 가치가 있다. 반대로 에이전트 하나만 간헐적으로 쓰고 터미널 관리가 큰 부담이 아니라면, 도구를 추가하는 편익부터 따져볼 만하다.

Herdr를 **“에이전트가 더 좋은 코드를 쓰게 해주는 모델”이 아니라 “여러 에이전트가 실행되는 환경을 관리하는 도구”**로 보면 역할이 선명해진다. 작업 공간과 상태를 한곳에 모으는 기능은 제공하지만, 코드 품질·작업 완료·배포 승인까지 대신 판단하는 것은 아니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-4">[4]</a>

처음 검토할 때는 공식 문서를 읽고 비민감한 작은 프로젝트에서 상태 감지, detach·reattach, 복구 조건을 각각 확인하는 접근을 권한다. 이 글에서는 설치나 실행을 수행하지 않았으며, 실제 환경에서의 안정성·성능은 별도 검증이 필요하다.

## Sources

[1] <span id="source-1"></span> <a href="https://herdr.dev">Herdr: the runtime coding agents run on</a>

[2] <span id="source-2"></span> <a href="https://herdr.dev/docs/session-state">Session state and restore | herdr</a>

[3] <span id="source-3"></span> <a href="https://herdr.dev/docs/agents">Agents | herdr</a>

[4] <span id="source-4"></span> <a href="https://herdr.dev/docs/socket-api">Socket API | herdr</a>

[5] <span id="source-5"></span> <a href="https://herdr.dev/docs/connecting-machines">Connecting machines | herdr</a>

[6] <span id="source-6"></span> <a href="https://github.com/herdrdev/herdr">GitHub - herdrdev/herdr: the runtime your coding agents live on · GitHub</a>
