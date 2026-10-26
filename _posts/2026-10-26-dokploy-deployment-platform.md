---
title: "Dokploy: 내 서버에 배포 플랫폼을 더하는 방법"
date: 2026-10-26 15:17:18 +0900
categories: [개발, 개발도구]
tags: [dokploy, docker, self-hosted, deployment, devops]
toc: true
comments: false
permalink: /posts/dokploy-deployment-platform/
---

앱을 컨테이너로 만드는 것과 꾸준히 운영하는 것은 다른 일이다. 소스를 받아 빌드하고, 도메인과 HTTPS를 연결하고, 로그를 확인하고, 데이터가 재배포 뒤에도 남는지 챙겨야 한다. 프로젝트가 늘수록 이 작업을 한곳에서 관리하고 싶어진다.

**Dokploy는 이런 배포·운영 작업을 웹 화면으로 묶는 자체 호스팅 가능한 플랫폼이다.** 공식 사이트와 문서는 애플리케이션, 데이터베이스, Docker Compose, 원격 서버 관리를 주요 기능으로 소개하며 Docker와 Traefik을 기반 기술로 설명한다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>

> 확인 기준: 2026년 10월 26일 KST. 공식 사이트·문서·GitHub 저장소를 읽고 작성한 소개글이다. Dokploy를 설치하거나 실제 서비스를 배포한 사용 후기는 아니며 성능·가용성·백업 복구를 실측하지 않았다.

## Dokploy는 무엇을 대신해 주나

GitHub 저장소는 Dokploy를 Heroku·Vercel·Netlify의 대안으로 소개한다. 다만 이 문구를 각 서비스의 기능·지원·운영 책임이 모두 같다는 뜻으로 받아들여서는 안 된다.<a href="#source-2">[2]</a>

Dokploy에서 주목할 부분은 **자신의 서버를 유지하면서 배포 관리 화면을 얻는다는 점**이다. 공식 기능에는 Dockerfile 및 빌드팩 계열 빌드, Compose 배포, 다중 서버, 로그·모니터링, 데이터베이스 관리가 포함된다.<a href="#source-1">[1]</a>

내가 해석한 적합한 사용 상황은 사이드 프로젝트 여러 개, 내부 서비스, 작은 팀의 웹앱·API처럼 배포 대상은 늘어나는데 운영 화면은 흩어져 있는 경우다. 반대로 서버 운영을 거의 맡고 싶지 않은 팀이라면 기능 목록보다 책임 범위를 먼저 비교하는 편이 좋다.

## 구조: 배포 관리와 실제 앱 실행을 구분하자

공식 아키텍처 문서는 Next.js 기반 관리 애플리케이션, 설정·운영 데이터를 보관하는 PostgreSQL, 리버스 프록시와 라우팅을 담당하는 Traefik을 설명한다. Cloud 문서는 관리 계층에 Redis도 포함해 소개한다.<a href="#source-4">[4]</a><a href="#source-9">[9]</a>

아래는 역할을 이해하기 위한 간략한 개념도다. 실제 네트워크 구성도나 제품 화면은 아니다.<a href="#source-8">[8]</a><a href="#source-9">[9]</a>

```text
관리 계층: Dokploy 화면 · 배포 설정 · 작업 조율
                     │ 배포·관리
                     ▼
실행 계층: 내 서버의 앱 · 데이터베이스 · Traefik
```

**관리 화면을 누가 운영하느냐와 앱 서버를 누가 운영하느냐는 별개의 질문**이다. 이 구분이 자체 호스팅과 Dokploy Cloud를 비교할 때 특히 중요하다.<a href="#source-8">[8]</a><a href="#source-9">[9]</a>

## 주요 기능: 앱뿐 아니라 Compose와 데이터까지

### 앱과 여러 서비스 묶음 배포

공식 사이트는 Dockerfile, Nixpacks, Heroku Buildpacks 등을 통한 애플리케이션 배포를 소개한다. Compose 문서는 일반 Docker Compose 방식과 Docker Swarm용 Stack 방식을 구분한다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>

이미 Compose로 구성한 앱·DB·캐시 묶음을 관리하고 싶다면 이 기능이 검토 지점이다. 다만 **Compose와 Stack은 완전히 같은 실행 방식이 아니다.** 문서는 Stack 모드에서 `build` 같은 일부 Compose 기능을 사용할 수 없다고 명시한다.<a href="#source-6">[6]</a>

환경변수도 주의해야 한다. UI에 입력한 값은 `.env` 파일에 저장되지만 컨테이너에 자동 주입되지는 않는다. Compose의 `env_file` 또는 `environment`에서 필요한 변수를 연결해야 한다.<a href="#source-6">[6]</a>

### 데이터베이스, 로그, 백업

Dokploy는 PostgreSQL·MySQL·MariaDB·MongoDB·Redis 관리와 로그·리소스 모니터링을 소개한다. 데이터베이스 문서에는 S3 목적지에 자동 백업을 설정하는 기능도 설명돼 있다.<a href="#source-1">[1]</a><a href="#source-10">[10]</a>

하지만 **백업 기능이 있다는 것과 내 데이터가 복구 가능하다는 것은 다르다.** 도입 시에는 백업 대상·저장 위치·보존 정책을 정하고, 별도 환경에서 복원해 보는 절차를 권한다. 이 글에서 복구를 시험한 것은 아니다.

특히 Compose의 Volume Backups는 문서상 Docker named volume에 적용되며 bind mount에는 적용되지 않는다. 영속 볼륨 밖의 파일은 재시작이나 새 배포 뒤 사라질 수 있으므로, “화면에서 파일이 보인다”는 이유만으로 영속성을 가정하면 안 된다.<a href="#source-6">[6]</a><a href="#source-10">[10]</a>

## Self-Hosted와 Cloud: 서버 비용까지 없어지는 것은 아니다

공식 비교 문서에 따르면 Self-Hosted는 Dokploy 관리 인스턴스의 가동과 업데이트를 직접 책임진다. Cloud는 Dokploy가 관리 계층을 운영하고 업데이트하지만, **앱과 데이터베이스는 여전히 사용자의 서버에서 실행된다.**<a href="#source-8">[8]</a><a href="#source-9">[9]</a>

따라서 Cloud를 “앱 실행용 서버까지 모두 제공하는 요금제”로 이해하면 비용과 책임을 잘못 계산할 수 있다. Cloud 사용료 외에 자신의 서버와 저장소 등 실제 운영 비용을 별도로 검토해야 한다.<a href="#source-9">[9]</a>

확인 시점의 Cloud 문서에는 Hobby가 서버당 월 **US$4.50**, Startup이 서버 3대를 포함해 월 **US$15**, 추가 서버는 월 **US$4.50**으로 안내돼 있다. 이는 문서에 표시된 월 요금이며, 실제 청구 조건·기능 제한은 가입 시점의 가격표를 다시 확인해야 한다. 이 글에서는 결제 화면을 확인하거나 가입하지 않았다.<a href="#source-9">[9]</a>

SSO·SCIM·감사 로그 같은 기업 기능도 기본 자체 호스팅과 동일하게 제공되는 것은 아니다. 공식 비교표는 Enterprise 계열과 기본 에디션의 차이를 따로 설명한다.<a href="#source-8">[8]</a>

## 도입 전에 꼭 확인할 네 가지

### 1. 서버 자원과 기존 서비스 충돌

설치 문서는 최소 **RAM 2GB·디스크 30GB**를 안내하고, 80·443·3000 포트가 사용 가능해야 한다고 명시한다. 이 수치를 여러 실제 앱을 운영하기에 충분한 용량 보장으로 읽으면 안 된다. 빌드 규모와 앱·DB 부하에 맞춘 용량 산정은 별도로 필요하다.<a href="#source-5">[5]</a>

특히 기존 웹 서버가 80·443을 쓰고 있다면 설치부터 실행하기보다 프록시·포트 구성을 먼저 검토하는 편이 안전하다. 이 글에는 운영 서버에 그대로 실행할 설치 명령을 넣지 않았다.<a href="#source-5">[5]</a>

### 2. 관리 화면과 배포 권한

Dokploy는 앱을 관찰하는 화면에 그치지 않고 배포·설정·데이터베이스 관리 권한을 가진다. 설치 문서는 HTTPS 도메인 설정과, 정상 접근을 먼저 확인한 뒤 IP·포트 직접 접근을 제한하는 절차를 안내한다.<a href="#source-5">[5]</a><a href="#source-10">[10]</a>

관리자 계정, SSH 키, 저장소·레지스트리 자격증명은 운영 자산으로 취급하는 것이 적절하다. Cloud도 SSH 연결 정보를 이용해 서버를 연결한다고 설명하므로, 편리함과 함께 접근 범위를 검토해야 한다.<a href="#source-9">[9]</a>

### 3. 업데이트와 버전 고정

설치 문서는 기본 설치 스크립트가 최신 안정 버전을 대상으로 한다고 설명한다. 특정 버전을 설치할 때는 해당 GitHub 릴리스에 첨부된 설치 스크립트를 쓰도록 안내하며, 개발용 canary와 안정 버전도 구분한다.<a href="#source-5">[5]</a>

저장소에서 현재 보이는 브랜치나 문서의 예시 버전을 곧바로 운영 권장 버전이라고 단정하지 않는 것이 좋다. 도입 시점의 릴리스와 변경사항을 확인하고, 업데이트 전 백업·복구 절차를 준비하자.

### 4. 라이선스는 저장소 전체를 한 단어로 단정하지 않기

확인한 `canary` 브랜치의 `LICENSE.MD`는 제한 대상 디렉터리 밖의 코드를 Apache License 2.0으로 설명하는 한편, `/proprietary` 디렉터리가 있으면 그 내용에는 `LICENSE_PROPRIETARY`가 적용된다고 명시한다. 따라서 **“저장소의 모든 기능이 동일한 Apache 라이선스”라고 요약하면 부정확하다.**<a href="#source-7">[7]</a>

이 글은 해당 라이선스 파일을 읽은 범위의 소개다. 배포할 특정 버전과 Enterprise 기능의 계약, 의존성 전체에 대한 라이선스 감사를 수행한 것은 아니다.<a href="#source-7">[7]</a>

## 누구에게 잘 맞을까

공식 기능과 운영 책임을 기준으로 보면, **서버 통제권은 유지하되 반복 배포 작업을 화면으로 정리하고 싶은 개발자·소규모 팀**이 검토할 만하다. Compose 기반 서비스를 운영하거나 여러 서버의 배포를 한곳에서 다루려는 경우에도 방향이 맞는다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a><a href="#source-8">[8]</a>

반대로 서버 보안·용량·복구 책임까지 외부에 맡기려는 팀이라면, Dokploy Cloud의 관리 계층 서비스와 완전 관리형 앱·DB 서비스를 구분해서 비교해야 한다.<a href="#source-8">[8]</a><a href="#source-9">[9]</a>

Dokploy의 가치는 “운영을 없애준다”보다 **“운영 작업을 정리하고 반복하기 쉽게 만든다”**는 관점에서 보는 편이 정확하다. 작은 비민감 프로젝트로 배포·환경변수·영속 데이터·복원을 확인한 뒤 범위를 넓히는 접근을 권한다.

## Sources

[1] <span id="source-1"></span> [Dokploy · Deploy your applications with ease](https://dokploy.com)

[2] <span id="source-2"></span> [Dokploy · GitHub repository](https://github.com/dokploy/dokploy)

[3] <span id="source-3"></span> [Dokploy Docs · Welcome to Dokploy](https://docs.dokploy.com/docs/core)

[4] <span id="source-4"></span> [Dokploy Docs · Architecture of Dokploy](https://docs.dokploy.com/docs/core/architecture)

[5] <span id="source-5"></span> [Dokploy Docs · Installation](https://docs.dokploy.com/docs/core/installation)

[6] <span id="source-6"></span> [Dokploy Docs · Docker Compose](https://docs.dokploy.com/docs/core/docker-compose)

[7] <span id="source-7"></span> [Dokploy · LICENSE.MD (canary)](https://github.com/Dokploy/dokploy/blob/canary/LICENSE.MD)

[8] <span id="source-8"></span> [Dokploy Docs · Cloud vs Self-Hosted](https://docs.dokploy.com/docs/core/differences)

[9] <span id="source-9"></span> [Dokploy Docs · Dokploy Cloud](https://docs.dokploy.com/docs/core/cloud)

[10] <span id="source-10"></span> [Dokploy Docs · Databases](https://docs.dokploy.com/docs/core/databases)
