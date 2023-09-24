---
title: 스프링 부트 프로젝트 만들기
author: June
date: 2023-09-09 15:50:00 +0900
categories: [강좌]
tags: [강좌]
toc: true
math: true
mermaid: true
comments: true
---

1. 프로젝트 생성
    - [spring initializr](https://start.spring.io)에 접속해 프로젝트를 생성한다.
        - Project
            - 빌드 시스템을 선택한다.
            - 추천: gradle
        - Language
            - 개발 언어를 선택한다.
        - Spring Boot
            - Spring Boot 버전을 선택한다.
            - 뒤에 Snapshot이나, M 등이 붙은 버전은 아직 안정화 되지 않은 버전이다.
            - 특별히 선호하는 버전이 있다면 선택한다.
            - 그런것이 아니라면 최신 GA(General Availability, 정식 릴리즈) 버전(버전 표기 뒤에 다른 표기가 없는 버전)을 추천한다.
        - Group
            - 전체를 포괄하는 이름(회사명, 개발자명 등).
            - 주로 com.{회사명, 개발자명 등}와 같이 사용한다.
        - Artifact
            - 버전을 제외한 Jar파일 이름.
            - 소문자만 사용한다.
        - Name
            - 프로젝트 명.
            - 일반적으로 Artifact와 동일하다.
        - Package name
            - 베이스 package(namespace)
            - {Group}.{artifact}로 자동 생성된다.
            - 특별한 경우가 아니라면 그대로 사용하면 된다.
        - Packaging
            - Jar, War 중 선택한다.
            - 특별한 경우가 아니라면 Jar을 사용하면 된다.
        - Java
            - 사용한 Java 버전 선택
            - 추천: 최신 LTS 버전
        - Dependencies
            - 의존성 추가
            - 명확하게 사용할 의존성이 있으면 추가한다.
            - API 등 Web을 사용할 것이라면 Spring Web을 추가한다.
            - 비워두고 생성한 후 나중에 추가해도 된다.
        - GENERATE
            - 설정한 내용으로 프로젝트가 생성된다.
            - zip 파일로 다운로드 된다.
        - EXPLORE
            - 설정한 내용으로 생성될 프로젝트의 구조를 볼 수 있다.
        - SHARE
            - 설정한 내용을 공유 할 수 있다.

    ![spring initializr](/posts/development-cource/spring-initializr.png)

    - `GENERATE` 버튼을 눌러 프로젝프를 다운로드 받습니다.

1. 프로젝트 실행
    - 생성하여 다운로드 받은 프로젝트를 압축을 푼다.
    - intellij 등 IDE에서 열거나 프로젝트 폴더에서 `./gradlew bootRun` 명령을 실행한다.
    - 정상적으로 실행되면 `http://localhost:8080`으로 접속하여 `Whitelabel Error Page`가 출력되는지 확인한다.
    - `Whitelabel Error Page`가 출력되면 정상적으로 실행된 것이다.
    - `./gradlew bootRun` 명령을 실행한 터미널에서 `Ctrl + C`를 눌러 종료한다.

    ![bootRun](/posts/development-cource/boot-run.png)
    ![Whitelabel](/posts/development-cource/whitelabel.png)

[Source Code](https://github.com/ijung/spring-boot-demo/tree/feature/init-demo-project)