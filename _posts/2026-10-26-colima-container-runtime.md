---
title: "Colima: Mac에서 컨테이너 실행 환경을 간결하게 관리하는 방법"
date: 2026-10-26 17:23:00 +0900
categories: [개발, 개발도구]
tags: [colima, docker, lima, macos, containers]
toc: true
comments: false
permalink: /posts/colima-container-runtime/
---

Mac에서 필요한 것이 컨테이너 실행과 익숙한 `docker` 명령이라면, 실행 환경을 어떻게 구성할지부터 선택하게 된다. 웹앱과 데이터베이스를 띄우고, 프로젝트별 환경을 나누고, 필요할 때만 로컬 Kubernetes를 켜고 싶은 개발자에게 Colima는 검토할 만한 도구다.

**Colima는 Lima 위에서 컨테이너 런타임을 적은 설정으로 사용할 수 있게 해 주는 CLI 중심 도구다.** Intel·Apple Silicon Mac과 Linux를 지원하며, Docker·containerd·Incus와 선택적 Kubernetes 구성을 제공한다. 이름도 Containers on Lima에서 왔다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

> 확인 기준: 2026년 10월 26일 KST. 공식 GitHub README와 문서를 바탕으로 작성한 소개글이다. 이 글을 위해 Colima를 설치하거나 컨테이너·GPU 성능을 실측하지 않았다. 아래 명령은 문서 기반 예시이며 이 환경에서 실행한 결과가 아니다.

## Docker를 대체한다기보다, Docker를 실행할 환경을 준비한다

Colima를 이해할 때는 **Docker CLI, 컨테이너 런타임, 가상 머신 관리**를 나누어 보는 것이 좋다. Colima는 Lima를 활용해 런타임이 동작할 환경을 준비한다. Docker 런타임을 선택하면 Mac의 Docker 클라이언트로 컨테이너를 다루며, Docker CLI는 별도로 필요하다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a>

아래는 macOS에서 Docker 런타임을 사용하는 경우의 간단한 역할도다. 실제 측정한 네트워크 구성도는 아니다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-4">[4]</a>

```text
Mac의 터미널
  ├─ colima 명령 → Lima 기반 VM의 시작·설정·중지
  └─ docker 명령 → 선택한 Docker context
                         ↓
                 VM 안의 Docker 런타임
                         ↓
                 앱 · DB · 개발용 컨테이너
```

따라서 “Docker 명령을 계속 쓰면서 실행 환경을 CLI로 관리하고 싶다”는 요구와 잘 맞는다. 반면 이 구조만으로 Docker Desktop의 모든 UI·통합 기능을 동일하게 대체한다고 단정할 수는 없다. 필요한 IDE, Compose, 빌드 도구와의 연결을 각각 확인하는 편이 정확하다.

## 시작은 짧지만, 클라이언트와 플러그인은 구분하자

공식 설치 문서는 Homebrew에서 Colima와 Docker CLI를 함께 설치하는 방법을 안내한다. 기본 런타임은 Docker이며, 시작 뒤 기존 Docker 명령을 사용할 수 있다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a>

```bash
# 패키지 설치 및 최초 시작 시 외부 다운로드가 발생할 수 있다.
brew install colima docker
colima start
colima status

# 이미지가 로컬에 없으면 레지스트리에서 내려받는다.
docker run hello-world
docker ps
```

여기서 놓치기 쉬운 부분은 **Docker CLI 설치가 Compose·Buildx 플러그인 준비까지 보장하는 것은 아니라는 점**이다. 공식 설치 문서는 `docker-compose`와 `docker-buildx`를 별도 설치하고 CLI 플러그인으로 연결하는 절차를 제공한다.<a href="#source-3">[3]</a>

이미 개발 도구가 설치된 Mac이라면 기존 설정을 덮어쓰기 전에 다음 명령으로 필요한 기능이 인식되는지 확인하는 접근을 권한다.

```bash
docker compose version
docker buildx version
docker context ls
```

이 글에서는 기존 플러그인 경로를 강제로 교체하는 명령을 기본 설치 흐름에 넣지 않았다. 플러그인이 없다면 공식 설치 문서의 해당 절을 확인하자.<a href="#source-3">[3]</a>

## 실제로 유용한 기능 세 가지

### 1. VM 자원을 명시적으로 조절하기

README가 안내하는 기본 VM 설정은 CPU 2개, 메모리 2GiB, 저장 공간 100GiB다. CPU·메모리·디스크는 시작 옵션이나 설정 파일로 조절할 수 있다. 이 값은 VM 설정이지 특정 앱의 성능이나 호스트의 실제 디스크 점유량을 측정한 수치가 아니다.<a href="#source-1">[1]</a>

공식 README의 자원 변경 예시는 다음과 같다. 실행 중인 작업이 있다면 중지의 영향을 먼저 확인해야 한다.<a href="#source-1">[1]</a>

```bash
colima stop
colima start --cpu 4 --memory 8
```

작은 서비스와 대규모 빌드, 로컬 DB는 필요한 자원이 다르다. 기본값을 정답처럼 쓰기보다 작업 부하에 맞춰 조정하는 것이 좋다.

### 2. 프로필로 개발 환경 나누기

Colima 프로필은 각자의 VM 자원·런타임·컨테이너·이미지·볼륨을 갖는 독립 인스턴스다. 설정 파일도 `~/.colima/<profile>/colima.yaml`로 분리된다.<a href="#source-5">[5]</a>

```bash
# 개발용 Docker 환경
colima start dev

# 별도의 containerd 환경
colima start containerd --runtime containerd

# 상태와 Docker 연결 대상 확인
colima list
docker context ls
```

Docker 프로필마다 context도 생성된다. 공식 문서는 `dev` 프로필의 예로 `colima-dev` context를 안내한다.<a href="#source-5">[5]</a>

```bash
docker context use colima-dev
```

프로필을 나누면 환경 구분은 쉬워지지만, 동시에 실행하는 VM의 자원 부담까지 사라지는 것은 아니다. 이름만 다르게 만들기보다 목적과 자원 한도를 함께 정하는 편이 좋다.

### 3. 필요할 때만 Kubernetes와 다른 런타임 사용하기

Colima는 k3s 기반 로컬 Kubernetes를 선택적으로 실행할 수 있다. `kubectl`은 별도 클라이언트이며, Kubernetes가 필요하지 않은 Docker 전용 사용도 가능하다.<a href="#source-1">[1]</a><a href="#source-3">[3]</a><a href="#source-4">[4]</a>

```bash
# 별도 프로필에 로컬 Kubernetes 환경 생성
colima start k8s --kubernetes
```

containerd에는 내장 `colima nerdctl` 래퍼를 사용할 수 있고, Incus 런타임도 지원한다. 다만 Incus 안에서 VM을 실행하는 기능은 중첩 가상화 조건이 있으며, README는 Apple Silicon의 경우 M3 이상이라는 제한을 명시한다.<a href="#source-1">[1]</a><a href="#source-4">[4]</a>

여러 Kubernetes 클러스터나 별도 구성이 필요하다면 Kind·k3d·minikube도 비교 대상이다. 공식 FAQ도 이런 요구에서는 해당 도구들이 더 적합할 수 있다고 설명한다.<a href="#source-2">[2]</a>

## Apple Silicon과 AI: 기본 컨테이너 사용과 별개의 조건

README는 Apple Silicon에서 Rosetta 2 에뮬레이션을 사용하는 설정을 소개한다. 해당 예시는 Colima v0.5.3 이상 및 macOS 13 이상을 조건으로 안내한다.<a href="#source-1">[1]</a>

```bash
colima start --vm-type=vz --vz-rosetta
```

이 옵션을 “모든 x86 이미지가 네이티브와 같은 성능·호환성으로 동작한다”는 보장으로 읽어서는 안 된다. 실제 사용할 이미지와 빌드 작업으로 확인할 부분이다.

GPU 가속 AI 워크로드는 또 다른 경로다. README는 Colima v0.10.0 이상, Apple Silicon, macOS 13 이상과 별도 설치한 krunkit을 조건으로 설명한다. 일반적인 `colima start`만으로 모든 컨테이너에 GPU가 제공된다는 뜻은 아니다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>

현재 README와 AI 전용 문서는 Docker Model Runner를 기본 runner로, Ramalama를 다른 선택지로 설명한다. 모델 레지스트리 지원과 이름 접두사도 runner에 따라 다르다.<a href="#source-1">[1]</a><a href="#source-6">[6]</a>

**문서 간 차이도 있었다.** FAQ의 AI 절은 HuggingFace를 기본 레지스트리로 설명하지만, README와 AI 전용 문서는 Docker AI Registry를 기본으로 안내한다. 이 글은 후자의 일치하는 설명을 소개하되, 설치할 릴리스와 `colima model --help`에서 실제 동작을 다시 확인할 것을 권한다. 모델 실행 예시를 검증 완료된 레시피처럼 제시하지 않는 이유다.<a href="#source-1">[1]</a><a href="#source-2">[2]</a><a href="#source-6">[6]</a>

## 도입 전에 확인할 운영상 주의점

### Docker context와 소켓 경로

Colima는 시작할 때 Docker context를 생성·활성화하므로 기존 Docker 환경과 함께 쓸 때 현재 연결 대상을 확인해야 한다. FAQ는 Docker Desktop과 context를 통한 공존이 가능하다고 설명한다.<a href="#source-2">[2]</a><a href="#source-4">[4]</a>

소켓 경로도 도구 연동에서 중요하다. FAQ는 v0.4.0 이후 기본 프로필의 경로를 `~/.colima/default/docker.sock`으로 안내하며, `colima status`로 확인하도록 한다. 모든 도구가 이 경로를 자동으로 찾는다고 가정하지 말자.<a href="#source-2">[2]</a>

문제가 생겼을 때 시스템 소켓을 강제로 연결하기보다, 해당 앱의 소켓 설정과 현재 context를 먼저 확인하는 접근을 권한다.

### 파일 공유는 Mac → VM → 컨테이너 경계를 지난다

호스트 경로를 컨테이너에 bind mount했는데 내용이 비어 있다면 VM에 그 경로가 공유됐는지 확인해야 한다. FAQ는 사용자 홈 밖의 경로에서 이런 문제가 발생할 수 있으며 Colima 설정의 `mounts`에 경로를 추가하는 방법을 설명한다.<a href="#source-2">[2]</a>

프로젝트에 필요한 디렉터리만 공유하고, 자격증명·개인 파일을 포함한 넓은 경로를 편의상 마운트하지 않는 편이 안전하다.

### 삭제·업데이트를 단순한 재시작처럼 다루지 않기

FAQ는 v0.9.0 이상에서 일반 `colima delete`와 데이터까지 제거하는 `colima delete --data`를 구분한다. 하지만 런타임별 복구 조건은 같지 않다. Runtimes 문서는 soft delete 후 Docker·containerd는 자동 복원, Kubernetes는 미지원, Incus는 복구 단계 필요로 안내한다.<a href="#source-2">[2]</a><a href="#source-4">[4]</a>

**데이터 보존 기능은 백업이나 복구 검증을 대신하지 않는다.** 특히 DB 볼륨이 있다면 버전·런타임·백업을 확인하고, 삭제를 기본 장애 해결 명령으로 복사해 실행하지 않는 편이 좋다.

업데이트 설명에도 차이가 있다. FAQ는 containerd 업데이트를 안내하지만, Runtimes 문서는 `colima update`의 containerd 지원이 현재 없다고 명시한다. 따라서 “모든 런타임이 같은 명령으로 업데이트된다”고 요약하지 않고, 사용 버전의 도움말·릴리스와 런타임별 지원 범위를 확인해야 한다.<a href="#source-2">[2]</a><a href="#source-4">[4]</a>

## 누구에게 잘 맞을까

공식 기능을 기준으로 보면 **Docker CLI 중심으로 일하고, VM 자원과 프로필을 직접 관리하려는 개발자**에게 잘 맞는 선택지다. 컨테이너 개발과 로컬 Kubernetes 실험을 구분하거나, 목적별로 환경을 나누고 싶은 경우에도 검토할 만하다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a>

반대로 GUI 중심의 통합 경험이나 특정 도구의 Docker Desktop 연동에 크게 의존한다면, 설치 가능 여부만으로 전환을 결정하지 않는 편이 좋다. context·소켓·플러그인·파일 공유·기존 데이터 이전을 작은 비민감 프로젝트에서 먼저 확인하자.

Colima의 핵심은 “언제나 더 빠르다”거나 “모든 것을 자동으로 해결한다”는 데 있지 않다. **컨테이너 실행 환경을 CLI와 명시적인 설정으로 관리하는 선택지**라는 데 있다. 이 글은 성능 비교나 전환 성공을 실측하지 않았으며, 저장소는 프로젝트 라이선스를 MIT로 표시한다. 함께 사용하는 구성요소와 이미지의 라이선스까지 일괄 보장하는 뜻은 아니다.<a href="#source-1">[1]</a>

## Sources

[1] <span id="source-1"></span> [Colima · GitHub README](https://github.com/abiosoft/colima)

[2] <span id="source-2"></span> [Colima Docs · FAQ](https://colima.run/docs/faq)

[3] <span id="source-3"></span> [Colima Docs · Installation](https://colima.run/docs/installation)

[4] <span id="source-4"></span> [Colima Docs · Runtimes](https://colima.run/docs/runtimes)

[5] <span id="source-5"></span> [Colima Docs · Profiles](https://colima.run/docs/profiles)

[6] <span id="source-6"></span> [Colima Docs · AI Workloads](https://colima.run/docs/ai)
