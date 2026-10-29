---
title: "Mac에 Dokploy 설치하기: Colima 전용 VM으로 시작하는 초보자 가이드"
date: 2026-10-29 11:04:00 +0900
categories: [개발, 개발도구]
tags: [dokploy, colima, docker, macos, self-hosted]
toc: true
comments: false
permalink: /posts/dokploy-mac-colima-installation/
---

Mac에서 Dokploy를 살펴보고 싶다면 **Colima로 전용 Linux VM을 만든 뒤, 그 VM 안에 Dokploy를 설치**하는 방법을 사용할 수 있습니다. Docker Desktop 내부에 설치하는 안내가 아닙니다. Colima가 Linux 실행 환경을 제공하고, Dokploy 설치 스크립트는 그 환경 안에서 실행합니다.<a href="#source-2">[2]</a><a href="#source-4">[4]</a><a href="#source-5">[5]</a>

> **검증 범위:** 2026년 10월 29일 공식 문서·공개 소스·설치 스크립트를 확인해 구성한 안내입니다. 작성 과정에서 Mac 실기기에 설치하거나 관리자 화면까지 동작 검증하지 않았습니다. 공식 Linux 설치법과 Colima 문서를 조합한 학습용 절차이며, Dokploy가 공식적으로 검증한 Mac 설치법이라는 뜻은 아닙니다. 설치 시점의 버전·네트워크 환경에 따라 추가 조정이 필요할 수 있습니다.

## 먼저 알아둘 구조

![Mac 브라우저가 Colima 전용 Linux VM의 Dokploy 3000번 포트에 접속하고, VM 내부에서 Postgres와 Traefik이 함께 실행되는 구조](/assets/img/posts/dokploy-mac-colima-installation/dokploy-mac-architecture.svg)

*직접 제작한 구조 설명용 개념도입니다. 실제 설치 화면이 아닙니다.*

- **Mac 터미널:** Colima 설치, VM 생성·시작·중지를 담당합니다.
- **Dokploy VM 내부:** Dokploy 설치와 Docker 상태 확인을 수행합니다.
- **Mac 브라우저:** VM 주소의 `3000`번 포트로 관리자 화면에 접속합니다.

Dokploy 설치 스크립트는 macOS(`Darwin`)와 `/.dockerenv`가 존재하는 일반 Docker 컨테이너에서의 실행을 차단합니다. Linux VM을 사용하는 이유입니다. 모든 종류의 컨테이너가 동일하게 차단되는 것은 아니며, 공식 문서는 Proxmox LXC의 별도 지원도 설명합니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>

> **중요:** 이 글은 **새로 만든 전용 VM**만 대상으로 합니다. 설치 스크립트에는 `docker swarm leave --force`가 포함되어 있습니다. 기존 Swarm을 탈퇴시키므로 운영 서버나 다른 작업용 Docker 환경에서 실행하지 마세요. 전용 VM은 작업을 분리하는 수단이지 보안을 전부 보장하는 장치는 아닙니다.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>

## 1. 준비 사항 확인

- Intel 또는 Apple Silicon Mac. 현재 Colima FAQ는 macOS 13 이상을 요구합니다.<a href="#source-4">[4]</a>
- Colima v0.6.0 이상. 공식 FAQ가 이 버전부터 기본 VM 이미지를 Ubuntu로 설명하므로, 오래된 Alpine 기반 버전은 이 안내의 대상이 아닙니다.<a href="#source-4">[4]</a>
- Homebrew. 없다면 [Homebrew 공식 홈페이지](https://brew.sh)의 설치 안내와 설치 후 터미널 설정을 먼저 완료합니다.<a href="#source-7">[7]</a>
- 인터넷 연결. 패키지·VM 이미지·컨테이너 이미지를 다운로드합니다.
- Mac 자체 작업에 필요한 여유 메모리와 디스크 공간.

이 글은 VM에 **CPU 4개, 메모리 4GiB, 디스크 60GiB**를 할당합니다. 공식 Dokploy 최소 요구사항은 RAM 2GB와 디스크 30GB지만, 실제 앱 빌드에 필요한 자원은 프로젝트마다 다릅니다. 아래 값은 실습 예시이지 모든 앱에 충분하다는 보장은 아닙니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>

**Mac 터미널**에서 확인합니다.

```sh
brew --version
```

`command not found: brew`가 나오면 다음 단계로 넘어가지 말고 Homebrew 설치·PATH 설정부터 확인하세요.

## 2. Colima와 Docker CLI 설치

**Mac 터미널**에서 실행합니다.

```sh
brew install colima docker
```

Colima 공식 문서의 Homebrew 설치 방식입니다. Docker CLI는 명령을 보내는 도구이며, 실제 Docker 실행 환경은 다음 단계에서 만들 VM에 있습니다.<a href="#source-5">[5]</a>

설치 후 버전을 확인합니다.

```sh
colima version
docker --version
colima list
```

`colima list`에 이미 `dokploy` 프로필이 있다면 **여기서 멈추고 그 환경의 용도부터 확인**하세요. 기존 프로필에 설치 스크립트를 다시 실행하거나, 이름을 비우려고 삭제하면 안 됩니다.

Docker Desktop을 이미 사용 중이어도 이 글에서는 기존 Docker 연결 대상을 바꾸지 않습니다. CLI 설치 중 기존 파일 충돌이 발생하면 강제 덮어쓰기 대신 기존 설치 경로를 확인하세요.

## 3. Dokploy 전용 VM 생성

**Mac 터미널**에서 실행합니다.

```sh
colima start dokploy \
  --runtime docker \
  --cpus 4 \
  --memory 4 \
  --disk 60 \
  --network-address \
  --activate=false
```

줄 끝의 `\`는 명령이 다음 줄까지 이어진다는 뜻입니다. 코드 블록 전체를 복사해 실행하세요.

- `dokploy`: 이 VM의 프로필 이름입니다.
- `--runtime docker`: Docker를 사용하는 VM을 만듭니다.
- `--cpus 4`, `--memory 4`, `--disk 60`: VM 자원을 지정합니다.
- `--network-address`: Mac에서 접근할 수 있는 VM IP 주소를 활성화합니다.
- `--activate=false`: 시작하면서 Mac의 기본 Docker context를 이 VM으로 바꾸지 않습니다.<a href="#source-4">[4]</a><a href="#source-6">[6]</a>

CPU 옵션은 확인한 공개 소스의 `--cpus`로 통일했습니다. 공식 README의 일부 예제에는 `--cpu`로 표기되어 있으므로, 설치된 버전에서 옵션 오류가 나면 `colima start --help`로 실제 지원 옵션을 확인하고 진행하세요.<a href="#source-5">[5]</a><a href="#source-6">[6]</a>

네트워크 설정에는 Mac 관리자 권한이 필요할 수 있습니다. 비밀번호 입력 중 문자가 표시되지 않는 것은 터미널의 일반적인 동작입니다. 권한 오류를 해결하려고 임의의 sudoers 수정이나 보안 기능 해제를 하지 마세요.<a href="#source-4">[4]</a>

시작 후 다시 확인합니다.

```sh
colima list
```

`dokploy` 행이 실행 상태이고 `ADDRESS`가 표시되는지 확인합니다. 이후 설치와 Docker 점검은 **Mac에서 바로 실행하는 것이 아니라 VM 안에서** 합니다.

## 4. VM 진입과 설치 전 점검

**Mac 터미널**에서 접속합니다.

```sh
colima --profile dokploy ssh
```

이제부터 **Dokploy VM 내부**입니다. 먼저 확인 명령만 실행합니다.

{% raw %}
```sh
uname -s
sudo docker info --format '{{.Swarm.LocalNodeState}}'
sudo docker ps -a
sudo ss -tulnp
```
{% endraw %}

- `uname -s`는 `Linux`여야 합니다. `Darwin`이면 아직 Mac이므로 설치하지 마세요.
- 새 Docker 환경의 Swarm 상태는 `inactive`가 기대됩니다. `active`이거나 오류가 나면 원인을 확인하기 전 설치하지 마세요.
- 기존 컨테이너가 보이면 정말 비어 있는 전용 VM인지 확인합니다.
- VM 내부의 `80`, `443`, `3000` 포트가 사용 중이면 설치를 진행하지 않습니다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>

`Linux`라는 출력 하나만으로 전용 VM임이 입증되는 것은 아닙니다. 앞서 `dokploy` 프로필로 접속했는지 함께 확인해야 합니다.

## 5. 공식 설치 스크립트 확인·실행

**Dokploy VM 내부**에서 다운로드합니다.

```sh
curl -fsSL https://dokploy.com/install.sh -o /tmp/dokploy-install.sh
```

**다운로드에 성공한 경우에만** 내용을 확인합니다.

```sh
less /tmp/dokploy-install.sh
```

방향키로 읽고 `q`로 나옵니다. 공식 주소에서 내려받았는지 확인하세요. 다운로드 성공 자체가 스크립트 안전성 검증은 아닙니다. 이 스크립트는 VM에서 관리자 권한으로 시스템을 변경합니다.

새로 만든 전용 VM임을 확인했다면 실행합니다.

```sh
sudo sh /tmp/dokploy-install.sh
```

공식 문서도 `sh` 실행 방식을 안내합니다. 확인 시점의 스크립트는 Docker Swarm·네트워크·Dokploy 및 Postgres 서비스·Traefik 컨테이너 등을 구성합니다. 기존 Swarm 강제 탈퇴가 포함되므로 다른 작업용 환경에 실행하지 마세요.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-3">[3]</a>

> 오류가 나면 **설치 스크립트를 즉시 재실행하지 마세요.** 일부 단계가 이미 반영됐을 수 있습니다. 실패한 명령과 로그부터 확인합니다. 이 글에서는 자동 복구·초기화·삭제를 안내하지 않습니다.

## 6. 서비스 상태 확인

계속 **Dokploy VM 내부**에서 실행합니다.

```sh
sudo docker service ls
sudo docker service logs --tail 50 dokploy
sudo docker ps --filter name=dokploy-traefik
```

확인 시점의 설치 스크립트 기준으로 다음을 살펴봅니다.<a href="#source-3">[3]</a>

- `dokploy`와 `dokploy-postgres` 서비스의 `REPLICAS`가 각각 `1/1`인지.
- Dokploy 로그에 반복되는 시작 실패나 데이터베이스 연결 오류가 없는지.
- `dokploy-traefik` 컨테이너가 실행 중인지. Traefik은 별도 컨테이너이므로 `docker service ls`에 없을 수 있습니다.

`1/1`은 작업 복제 수가 맞는다는 뜻이지 관리자 화면과 앱 배포까지 정상이라는 뜻은 아닙니다. 처음에는 이미지 다운로드와 초기화 때문에 시간이 필요할 수 있습니다. 계속 준비되지 않으면 다음 읽기 전용 명령으로 원인을 확인합니다.

```sh
sudo docker service ps --no-trunc dokploy
sudo docker service ps --no-trunc dokploy-postgres
sudo docker logs --tail 50 dokploy-traefik
```

로그를 다른 사람에게 공유할 때는 비밀번호·토큰·접속 문자열·개인정보를 가리세요.

VM에서 나옵니다.

```sh
exit
```

## 7. Mac 브라우저로 관리자 화면 접속

다시 **Mac 터미널**에서 실행합니다.

```sh
colima list
```

`dokploy` 행의 `ADDRESS`를 복사해 브라우저 주소창에 입력합니다.

```text
http://<ADDRESS>:3000
```

`<ADDRESS>`는 그대로 입력하는 문자열이 아닙니다. 예를 들어 실제 표시된 주소가 `192.168.106.2`라면 `http://192.168.106.2:3000`으로 입력합니다. **이 IP는 설명용 예시**이므로 자신의 출력값을 사용하세요.

설치 스크립트가 출력한 공인 IP나 다른 Docker 환경의 `localhost:3000`을 무조건 사용하지 않습니다. 이 절차에서는 Colima의 접근 가능한 VM 주소를 기준으로 확인합니다.<a href="#source-3">[3]</a><a href="#source-4">[4]</a>

초기 설정 화면이 열리면 관리자 계정을 생성합니다. 공식 문서의 포트 역할은 다음과 같습니다.<a href="#source-1">[1]</a>

- `3000`: Dokploy 관리 화면
- `80`: Traefik을 통한 HTTP 앱 트래픽
- `443`: Traefik을 통한 HTTPS 앱 트래픽

관리자 계정은 접속 즉시 만들고 다른 서비스와 비밀번호를 재사용하지 마세요. 신뢰할 수 있는 로컬 네트워크에서 실습하고, 이 실습을 위해 공유기 포트포워딩이나 인터넷 공개를 설정하지 마세요. 포트포워딩을 하지 않았다는 사실만으로 모든 다른 기기의 접근이 차단된다고 단정할 수도 없습니다.

이 글의 HTTP 주소는 로컬 초기 접속용입니다. 인터넷 공개 운영에서는 HTTPS와 접근 통제 등을 별도로 설계해야 합니다. 공식 설치 문서도 HTTPS 구성을 권고합니다. **이 글을 마쳤다고 외부 공개 운영 준비가 끝난 것은 아닙니다.**<a href="#source-1">[1]</a>

## 8. 다음 날 다시 사용하는 방법

**Mac 터미널**에서 중지합니다.

```sh
colima stop dokploy
```

다시 사용할 때는 같은 프로필을 시작합니다.

```sh
colima start dokploy --activate=false
colima list
```

VM 정지·시작과 Dokploy 재설치는 다릅니다. 다시 켰다고 설치 스크립트를 재실행하지 마세요. `ADDRESS`를 다시 확인하고 필요하면 VM에 접속해 앞의 서비스 상태 점검을 반복합니다.<a href="#source-4">[4]</a><a href="#source-5">[5]</a>

Mac 절전·종료 중에도 계속 서비스를 제공하는 상시 서버로 생각하면 안 됩니다. 이 글의 권장 용도는 로컬 학습·기능 탐색입니다.

또한 **정지는 백업이 아닙니다.** 설치 스크립트에는 Docker 볼륨뿐 아니라 `/etc/dokploy`의 설정 파일도 포함됩니다. Colima 버전에 따라 삭제 시 보존 범위가 다르고, 최신 FAQ의 컨테이너 데이터 보존 설명도 모든 설정의 보존을 보장하지 않습니다. `colima delete`, `docker system prune`, 볼륨 삭제를 문제 해결 첫 단계로 사용하지 마세요.<a href="#source-3">[3]</a><a href="#source-4">[4]</a>

## 막혔을 때 확인할 순서

### `This script must be run on Linux`

Mac 터미널이나 일반 Docker 컨테이너에서 실행했을 가능성이 있습니다. 설치를 멈추고 `colima --profile dokploy ssh`로 진입했는지와 `uname -s`를 확인하세요.<a href="#source-3">[3]</a>

### 관리자 화면이 열리지 않음

1. Mac의 `colima list`에서 `dokploy` 실행 상태와 현재 `ADDRESS` 확인.
2. VM에서 Dokploy·Postgres 서비스의 복제 수와 로그 확인.
3. 브라우저 주소가 `http://실제주소:3000`인지 확인.
4. 서비스는 실행 중인데 접속이 안 되면 VPN·방화벽·VM 네트워크 구성을 확인. 방화벽을 통째로 끄거나 임의 포트포워딩을 추가하지 않기.

### IP 자동 감지 또는 Swarm 초기화 오류

현재 설치 스크립트는 Swarm에 알릴 주소로 사설 IP를 우선 찾고, 없으면 공인 IP 조회로 넘어갑니다. 공식 수동 설치 문서에 실린 예제 코드와 현재 내려받는 스크립트의 세부 동작은 다를 수 있으며, 이 설명은 현재 스크립트를 기준으로 합니다. `ADVERTISE_ADDR`로 수동 지정할 수 있지만, 이는 단순히 브라우저에 입력할 주소를 바꾸는 옵션이 아닙니다. VM 인터페이스와 Swarm 상태를 먼저 확인해야 합니다. 부분 설치 상태에서 무작정 다시 실행하지 마세요.<a href="#source-2">[2]</a><a href="#source-3">[3]</a>

공식 예시에서 `sudo`를 사용할 경우 환경변수는 `sudo ADVERTISE_ADDR=... sh ...`처럼 전달합니다. 앞서 `export`만 했다고 sudo 뒤에도 전달된다고 가정하면 안 됩니다. 초보자 단계에서 이 수정이 필요해졌다면 로그와 네트워크 상태를 바탕으로 도움을 요청하는 편을 권합니다.<a href="#source-2">[2]</a>

### 이미지 또는 패키지 다운로드 실패

인터넷 연결·DNS·프록시·레지스트리 접근 문제를 구분하세요. 설치 도구의 보안 검증을 끄거나 출처 불명 미러로 바꾸지 않습니다. Apple Silicon이라면 배포하려는 앱 이미지의 ARM 지원도 별도로 확인해야 합니다. Colima의 Mac 지원과 모든 앱 이미지의 호환성은 같은 의미가 아닙니다.

## 마무리 체크리스트

- [ ] 다른 Docker 작업용 환경이 아닌 새 `dokploy` VM을 사용했다.
- [ ] 설치는 Mac이 아니라 Linux VM 안에서 실행했다.
- [ ] 서비스·Traefik 컨테이너 상태를 확인했다.
- [ ] 현재 VM 주소로 관리자 화면에 접속하고 계정을 생성했다.
- [ ] 이 구성은 로컬 실습용이며 HTTPS·외부 공개 운영 설정과 별개임을 이해했다.
- [ ] 다음 사용 시 재설치 대신 같은 프로필을 시작한다.

Dokploy 자체의 기능과 운영 책임은 [기존 Dokploy 소개글](https://ijung.github.io/posts/dokploy-deployment-platform/)에서 먼저 살펴볼 수 있습니다.

### 자료 확인 기록

초안 자료 확인: 2026-10-29 11:04 KST. 게시 전 공식 자료 재조회: 같은 날 11:45 KST. 공식 문서와 `main` 소스, 설치 스크립트는 이후 변경될 수 있습니다. 재조회한 스크립트의 SHA-256은 `d0132d675253770ede147ce636ad7b0e40c69d8698d7ebc8e9f3f196621b8eaa`이며 초안 조사본과 일치했습니다. 이는 이번 조사 자료 식별값이며 향후 다운로드의 안전성 보증이나 고정 설치 버전은 아닙니다.

공식 문서·공개 소스의 서술과 명령 구성을 대조한 안내입니다. 실제 Mac에서의 VM 생성·Dokploy 설치·재시작·관리자 화면 동작은 검증하지 않았습니다.

## Sources

[1] <span id="source-1"></span> [Dokploy · Installation](https://docs.dokploy.com/docs/core/installation)

[2] <span id="source-2"></span> [Dokploy · Manual Installation](https://docs.dokploy.com/docs/core/manual-installation)

[3] <span id="source-3"></span> [Dokploy · Current install.sh](https://dokploy.com/install.sh)

[4] <span id="source-4"></span> [Colima · FAQ (main)](https://raw.githubusercontent.com/abiosoft/colima/main/docs/FAQ.md)

[5] <span id="source-5"></span> [Colima · Official repository](https://github.com/abiosoft/colima)

[6] <span id="source-6"></span> [Colima · start command source (main)](https://raw.githubusercontent.com/abiosoft/colima/main/cmd/start.go)

[7] <span id="source-7"></span> [Homebrew · Official installation guide](https://brew.sh)
