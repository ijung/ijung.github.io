---
title: 'Raspberry Pi OS도 리눅스인데, 무엇이 다를까?'
date: 2026-11-06 07:30:00 +0900
categories: [Development, Raspberry Pi]
tags: [raspberry-pi-os, linux, debian, raspbian, ubuntu, beginner]
toc: true
comments: false
permalink: /posts/raspberry-pi-os-linux-differences/
---

**난이도: 입문 — 리눅스 설치 경험 없이 읽을 수 있습니다.**

> 확인 기준: **2026년 11월 6일(KST)**. 공식 문서 기반 해설이며 실장비 성능·호환성을 시험한 글이 아닙니다.

**Raspberry Pi OS는 Raspberry Pi에 맞춰 구성한 Debian 기반 리눅스 배포판입니다.**<a href="#source-5">[5]</a> 차이는 “리눅스인가”보다 **무엇을 묶고 어떤 보드를 지원하는가**에 있습니다.

하드웨어는 [라즈베리 파이 소개](/posts/raspberry-pi-introduction/), 설치 과정은 [OS 시작하기](/posts/raspberry-pi-os-getting-started/)에서 다룹니다. 여기서는 설계와 선택 기준을 살펴봅니다.

## 1. 출발점은 새 OS가 아니라, 아이들이 직접 만질 컴퓨터였습니다

Raspberry Pi의 배경에는 **컴퓨터를 사용하는 경험과 프로그램을 만드는 경험 사이의 간극**이 있었습니다. 재단의 전략문서는 프로젝트의 계기를 2000년대 초 Cambridge 컴퓨터과학 과정 지원자 감소에서 찾습니다. 프로그램 사용법은 배우지만 만드는 경험은 부족하고, 가정의 기기도 창작보다 소비를 유도한다는 문제의식이었습니다. 이는 재단이 설명한 배경이지, 모든 학교·학생의 상황을 입증한 통계는 아닙니다.<a href="#source-15">[15]</a>

Cambridge가 기록한 공동창립자 **Eben Upton**의 회고도 같습니다. 적절한 시기에 젊은 사람들에게 프로그래밍 가능한 하드웨어를 건네자는 구상이었습니다. **2006년 첫 프로토타입** 이후 팀은 저렴하고, 프로그래밍할 수 있고, 재미있고 튼튼한 컴퓨터를 만들려 했습니다. 교과서 한 권 정도인 **25달러를 목표 가격**으로 삼았다는 설명도 있습니다. 모든 모델의 실제 판매가격이 25달러였다는 뜻은 아닙니다.<a href="#source-14">[14]</a>

출발점은 “독자 OS를 만들자”보다 **자기 컴퓨터에서 코드를 실행해 볼 기회를 넓히자**였습니다. 재단도 접근성과 오픈소스를 강조하면서 Pi만이 컴퓨팅 교육의 유일한 답은 아니라고 명시했습니다.<a href="#source-15">[15]</a> 그다음 과제가 실제 보드에서 쓸 소프트웨어였습니다.

## 2. ‘ARM용 Debian’이면 끝은 아니었습니다

첫 **256MB Raspberry Pi Model B**는 **2012년 2월 29일** 출시됐습니다.<a href="#source-16">[16]</a> 메모리부터 작은 컴퓨터였지만, Raspbian의 탄생을 메모리 부족만으로 설명하면 핵심을 놓칩니다. 구체적인 간극은 **CPU의 명령어와 패키지의 빌드 대상**에 있었습니다.

개발자 **Mike Thompson**은 당시 Pi를 **ARMv6+VFP**, 일반 Debian Wheezy `armhf`의 지원 대상을 **ARMv7+VFPv3-D16+Thumb-2**로 구분했습니다.<a href="#source-13">[13]</a> 모두 ARM이라고 같은 바이너리를 실행할 수 있는 조합은 아니었습니다. **‘ARM 지원’과 ‘그 Pi에서 실행 가능’은 다른 조건**이었던 셈입니다.

그렇다고 “Debian이 전혀 실행되지 않았다”는 뜻은 아닙니다. 뒤의 공식 발표는 Raspbian 이미지를 **기존 Debian Squeeze 이미지를 대체하는 권장 설치본**이라고 설명합니다.<a href="#source-12">[12]</a> 이미 동작하는 경로는 있었지만, Pi의 부동소수점 하드웨어를 활용하는 패키지 구성을 갖출 과제가 남아 있었습니다.<a href="#source-13">[13]</a>

Raspbian은 Debian Wheezy `armhf`의 컴파일 설정을 ARMv6에 맞춘 포트였습니다. **hard-float**는 이 부동소수점 하드웨어 활용과 코드·호출 규약에 관련된 용어입니다.<a href="#source-2">[2]</a><a href="#source-13">[13]</a> 새 Linux 생태계를 처음부터 만드는 대신 **Debian 소프트웨어를 보드에 맞게 다시 빌드·배포하는 문제를 풀었다**고 정리할 수 있습니다. 모든 앱이 더 빠르다는 성능 보장은 아닙니다.

## 3. 커뮤니티 Raspbian이 공식 권장 이미지의 기반이 되기까지

**2012년 3월**, Thompson은 Debian 메일링리스트에 Pi용 포트 개발에 관심이 있다며 필요한 지식·장비·협력자를 질문했습니다. 기존 `armhf` 작업을 활용하면 소규모 자원봉사 팀도 운영할 수 있으리라는 구상이었습니다.<a href="#source-17">[17]</a> 완성된 공식 제품이 처음부터 내려온 것이 아니라, 오픈소스 기반 위에서 해법을 찾던 과정입니다.

주요 개발자는 **Mike Thompson과 Peter Green**입니다.<a href="#source-2">[2]</a><a href="#source-13">[13]</a> Raspbian 홈페이지는 초기 패키지 빌드가 **2012년 6월** 완료됐다고 기록하며, 재단과 별개인 커뮤니티 프로젝트임을 명시합니다.<a href="#source-18">[18]</a> **커뮤니티의 패키지 포트**와 **재단의 권장 SD 카드 이미지**를 구분해야 하는 이유입니다.

**2012년 7월**, Raspberry Pi는 첫 Raspbian 기반 SD 카드 이미지를 발표하고 기존 Squeeze를 대신할 권장 설치본이라고 밝혔습니다. 발표문은 Raspbian 개발자들을 소개하고, 이미지의 펌웨어·커널·응용 프로그램 개선을 설명합니다.<a href="#source-12">[12]</a> Thompson도 같은 달 공식 권장 채택 소식을 Debian 메일링리스트에 알렸습니다.<a href="#source-13">[13]</a>

즉 **커뮤니티가 Pi에 맞는 Debian 패키지 기반을 만들고, 공식 이미지가 이를 채택해 배포한 관계**입니다.<a href="#source-12">[12]</a><a href="#source-13">[13]</a> 재단이 처음부터 혼자 만든 새 커널도, 기존 Debian과 무관한 OS도 아니었습니다.

## 4. 보드가 늘면서 호환성과 사용성도 과제가 됐습니다

**64비트 CPU가 나왔다고 공식 OS가 곧바로 전부 64비트가 된 것은 아닙니다.** 2016년 Pi 3 발표는 64비트 CPU를 소개하면서도 기존 Pi와 같은 **32비트 Raspbian 사용자 공간**을 사용한다고 설명했습니다. 이후 공식 64비트 OS 설명도 기존 구성을 유지한 이유로 기기 간 호환성과 혼란 방지를 듭니다.<a href="#source-16">[16]</a><a href="#source-4">[4]</a>

**2020년에는 공식 32·64비트 이미지의 이름을 Raspberry Pi OS로 정하고, Debian arm64 기반 64비트 베타를 발표**했습니다. 명칭 변경의 설명은 새 사용자가 권장 OS를 쉽게 찾도록 하려는 것이었습니다.<a href="#source-3">[3]</a> **2022년에는 64비트판을 더 넓은 사용자에게 제공**한다고 발표했습니다.<a href="#source-4">[4]</a> 단순히 이름만 달라진 하나의 패키지 계보로 읽으면 안 됩니다.

현재 공식 문서도 32비트의 `raspbian.sources`, 64비트의 `debian.sources`, 공통 Pi 저장소 `raspi.sources`를 구분합니다.<a href="#source-6">[6]</a> 다음은 이 관계를 단순화한 개념도입니다.

![Debian에서 Raspbian을 거치는 32비트와 Debian arm64 기반 64비트 계보, 공통 커널·도구와 보드 통합 구성을 구분한 직접 제작 개념도입니다.](/assets/img/posts/raspberry-pi-os-linux-differences/lineage-and-layers.svg)
_직접 제작 개념도입니다. 전체 패키지 유래나 부팅 순서는 아닙니다. 근거: 공식 자료.<a href="#source-2">[2]</a><a href="#source-3">[3]</a><a href="#source-6">[6]</a> [원본 도식](/assets/img/posts/raspberry-pi-os-linux-differences/lineage-and-layers.svg)_

이후의 변화도 이름에 그치지 않았습니다. 공식 설명에 따르면 **2023년 Bookworm은 Pi 4·5에 Wayland를 기본 적용하고 저성능 모델에는 X를 유지**했습니다. 후속 릴리스에서는 `labwc`를 도입해 전 모델의 기본 Wayland 구성으로 전환한다고 발표했습니다.<a href="#source-9">[9]</a> **Trixie**에서는 설정 도구를 **Control Centre**로 통합하고, Lite·데스크톱 구성을 조합하기 쉽게 패키지를 모듈화했습니다.<a href="#source-8">[8]</a>

초기의 CPU별 패키지 문제에서 **보드 세대를 잇는 지원, 설정·화면·기본 도구**로 과제가 넓어졌다고 읽을 수 있습니다. 이는 위 전환점을 연결한 필자의 해석이지 모든 장치 조합의 무결함이나 성능 우월성 주장이 아닙니다.

## 5. 그래서 ‘같은 Linux’여도 구성이 다릅니다

Debian은 **커널**을 운영체제의 중심으로 설명하며 그 위에 도구·앱·APT 패키지 관리를 제공합니다.<a href="#source-1">[1]</a> 커널이 중심 설비라면 배포판은 메뉴·공급·운영 방식까지 갖춘 식당에 비유할 수 있습니다. Raspberry Pi OS는 이 기반에 보드용 소프트웨어·설정·기본 구성을 더합니다.<a href="#source-5">[5]</a><a href="#source-6">[6]</a>

**Pi 전용 OS만 실행할 수 있어서 반드시 필요한 것은 아닙니다.** 공식 자료도 타사 OS를 설명하며 Ubuntu 역시 지원 모델용 설치 경로를 제공합니다.<a href="#source-4">[4]</a><a href="#source-10">[10]</a> **ARM은 이 OS만의 특징이 아닙니다.** Debian arm64 기반도 있습니다.<a href="#source-3">[3]</a> 따라서 질문은 “Pi OS 대신 리눅스를 쓸까?”가 아니라 **“이 보드와 용도에 어떤 Linux 배포판의 구성·지원 경로를 선택할까?”**입니다.

## 6. 역사에서 이어지는 네 가지 설계 관점

아래에서 **공식 사실**과 **필자 해석**을 구분합니다.

- **교육·입문:** 이름 변경의 공식 설명은 새 사용자가 권장 OS를 쉽게 찾게 하려는 것이었습니다.<a href="#source-3">[3]</a> Desktop에는 Thonny, Full에는 Scratch 등이 포함됩니다.<a href="#source-5">[5]</a> **해석:** 첫 도구 선택의 부담을 줄이는 구성입니다. 학습 효과를 실측했다는 뜻은 아닙니다.
- **자원 제약:** Lite는 그래픽 데스크톱 없는 구성으로 헤드리스·임베디드·구형 모델 용도가 안내됩니다.<a href="#source-5">[5]</a> **해석:** 필요 없는 화면 구성을 빼는 선택입니다. 모든 작업에서 더 빠르거나 더 쉽다는 보장은 아닙니다.
- **하드웨어 통합:** 공식 지원 소프트웨어는 그래픽·카메라·오디오 등을 포함합니다.<a href="#source-6">[6]</a> **해석:** 보드 기능을 쓰는 경로를 한데 연결하는 가치입니다. 모든 외부 장치의 자동 지원을 뜻하지 않습니다.
- **안정성·새 기술:** Debian 출시 주기를 시차를 두고 따르지만, Trixie에서는 Control Centre와 모듈화한 데스크톱 패키지 구성도 도입했습니다.<a href="#source-5">[5]</a><a href="#source-8">[8]</a> **해석:** 안정적인 기반과 필요한 변화를 절충합니다. 무장애나 최신 기능의 즉시 반영을 보장하지는 않습니다.

## 7. Debian·Ubuntu와 같은 것, 다른 것

| 지점 | 이해할 차이 |
| --- | --- |
| 패키지 | APT 경험을 활용할 수 있지만 같은 `.deb`라고 다른 배포판 패키지를 무조건 섞을 수는 없습니다.<a href="#source-1">[1]</a><a href="#source-5">[5]</a><a href="#source-11">[11]</a> |
| 터미널·권한 | Pi OS도 터미널과 관리 작업의 `sudo`를 사용합니다. 전부 별개인 명령 체계는 아닙니다.<a href="#source-5">[5]</a><a href="#source-7">[7]</a> |
| 보드·부팅 | ARM 지원만으로 특정 보드의 부팅까지 보장되지는 않습니다. 실제 모델·이미지 지원 조합을 확인합니다.<a href="#source-5">[5]</a><a href="#source-10">[10]</a> |
| 장치 설정 | Pi OS는 GUI·`raspi-config`·`/boot/firmware/config.txt` 등의 설정 경로를 제공합니다. 다른 배포판의 메뉴와 동일하다고 가정하지 않습니다.<a href="#source-7">[7]</a> |
| 기본 도구·화면 | Desktop·Full·Lite부터 포함 앱과 화면 유무가 다릅니다.<a href="#source-5">[5]</a> |

**시점도 중요합니다.** 확인 시점의 공식 문서는 최신 계열을 Trixie, 이전을 Bookworm으로 안내합니다.<a href="#source-5">[5]</a> Trixie의 Control Centre와 Bookworm의 Raspberry Pi Configuration도 구분됩니다.<a href="#source-7">[7]</a><a href="#source-8">[8]</a> 오래된 가이드는 OS 계열부터 맞춰 읽는 편이 좋습니다.

## 8. 용도별 선택 기준

다음은 공식 구성·지원 자료를 바탕으로 한 **필자의 제안**이며 성능 순위가 아닙니다.<a href="#source-5">[5]</a><a href="#source-10">[10]</a><a href="#source-11">[11]</a>

- **첫 프로그래밍·전자장치 학습:** 공식 보드 설정 경로를 따르려면 Pi OS Desktop을 검토합니다.
- **모니터 없는 서버·센서 수집:** Pi OS Lite를 검토하되 장치·패키지 지원을 먼저 확인합니다.
- **기존 Ubuntu 운영 방식 유지:** 해당 Pi 모델을 지원하는 Ubuntu Server를 검토합니다.
- **Debian 자체 학습:** Debian을 검토하되 보드 부팅·장치 지원을 별도로 확인합니다.
- **기존 프로젝트 유지:** 최신 이름보다 확장 보드·라이브러리가 지원하는 OS 계열을 우선합니다.

**배포판 / Desktop·Lite / 32·64비트 / Trixie 같은 기반 계열은 서로 다른 조건**입니다.<a href="#source-3">[3]</a><a href="#source-5">[5]</a><a href="#source-6">[6]</a> 하나를 골랐다고 나머지까지 자동으로 정해지지는 않습니다.

결론은 간단합니다. **같은 Linux 계열이어도 보드 통합·기본 구성·지원 경로가 다릅니다.** 실장비 테스트나 성능 우월성을 주장하는 글은 아닙니다. 설치가 필요하면 [첫 설정 가이드](/posts/raspberry-pi-os-getting-started/)로 이어가면 됩니다.

## Sources

<p id="source-1">[1] <a href="https://www.debian.org/intro/about">Debian · About Debian</a></p>

<p id="source-2">[2] <a href="https://raspbian.org/RaspbianAbout">Raspbian · About Raspbian</a></p>

<p id="source-3">[3] <a href="https://www.raspberrypi.com/news/8gb-raspberry-pi-4-on-sale-now-at-75">Raspberry Pi · 8GB Raspberry Pi 4 on sale now at $75</a></p>

<p id="source-4">[4] <a href="https://www.raspberrypi.com/news/raspberry-pi-os-64-bit">Raspberry Pi · Raspberry Pi OS (64-bit)</a></p>

<p id="source-5">[5] <a href="https://www.raspberrypi.com/documentation/computers/os.html">Raspberry Pi · Raspberry Pi OS</a></p>

<p id="source-6">[6] <a href="https://www.raspberrypi.com/documentation/computers/software-sources.html">Raspberry Pi · Software sources</a></p>

<p id="source-7">[7] <a href="https://www.raspberrypi.com/documentation/computers/configuration.html">Raspberry Pi · Configuration</a></p>

<p id="source-8">[8] <a href="https://www.raspberrypi.com/news/trixie-the-new-version-of-raspberry-pi-os">Raspberry Pi · Trixie — the new version of Raspberry Pi OS</a></p>

<p id="source-9">[9] <a href="https://www.raspberrypi.com/news/a-new-release-of-raspberry-pi-os">Raspberry Pi · A new release of Raspberry Pi OS</a></p>

<p id="source-10">[10] <a href="https://ubuntu.com/download/raspberry-pi">Ubuntu · Install Ubuntu on a Raspberry Pi</a></p>

<p id="source-11">[11] <a href="https://www.debian.org/doc/manuals/debian-faq/choosing.en.html">Debian · Chapter 3. Choosing a Debian distribution</a></p>

<p id="source-12">[12] <a href="https://www.raspberrypi.com/news/raspbian-based-sd-card-image-released">Raspberry Pi · Raspbian-based SD card image released</a></p>

<p id="source-13">[13] <a href="https://lists.debian.org/debian-arm/2012/07/msg00041.html">Mike Thompson · The new armhf in town...</a></p>

<p id="source-14">[14] <a href="https://www.cam.ac.uk/stories/raspberrypi">University of Cambridge · The life of Pi: Ten years of Raspberry Pi</a></p>

<p id="source-15">[15] <a href="https://static.raspberrypi.org/files/about/RaspberryPiFoundationStrategy2016-18.pdf">Raspberry Pi Foundation · Strategy 2016–2018</a></p>

<p id="source-16">[16] <a href="https://www.raspberrypi.com/news/raspberry-pi-3-on-sale">Raspberry Pi · Raspberry Pi 3 on sale now at $35</a></p>

<p id="source-17">[17] <a href="https://lists.debian.org/debian-arm/2012/03/msg00002.html">Mike Thompson · Questions regarding armhf port for Raspberry Pi</a></p>

<p id="source-18">[18] <a href="https://www.raspbian.org">Raspbian · Welcome to Raspbian</a></p>
