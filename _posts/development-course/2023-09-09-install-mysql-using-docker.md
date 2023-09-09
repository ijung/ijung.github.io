---
title: 도커를 이용해 mysql 설치하기
author: June
date: 2023-09-09 14:30:00 +0900
categories: [강좌]
tags: [강좌]
toc: true
math: true
mermaid: true
comments: true
---

1. 도커 설치
[도커 홈페이지](https://www.docker.com)의 [Get Docker](https://docs.docker.com/get-docker/)를 통해 도커를 설치한다.

    ![Get Docker](/posts/development-cource/screencapture-docs-docker-get-docker.png)
  
    - windows나 mac의 경우 docker desktop을 설치하면 된다.
    - 설치 후 재부팅을 해야 할 수도 있다.
    - windows의 경우 wsl kenel update를 요구하는 경우도 있다.
        - 이 경우 cmd 혹은 PowerShell에서 `wsl --update`를 입력한다.
    - 이 후 모든 명령은 따로 언급이 없다면 windows의 경우 cmd 혹은 PowerShell에서(반드시 `관리 권한으로 실행`해야 한다.) mac이나 linux는 terminal에서 실행한다.

1. 도커가 정상적으로 설치되었는지 확인 한다.

    ```powershell
    docker --version
    ```

    - 버전이 출력되면 정상적으로 설치된 것이다.

    ![docker version](/posts/development-cource/docker-version.png)

1. mysql 이미지를 pull한다.

    ```powershell
    docker pull mysql
    ```

    - 버전을 명시해 주지 않으면 lastest 버전을 받는다.
    - 특정 버전을 받기를 원할 경우 `docker pull mysql:8.0.22`와 같이 실행한다.
    - pull 이미지는 [docker hub](https://hub.docker.com)에서 확인이 간능하다.
        - [docker hub: mysql](https://hub.docker.com/_/mysql)

    ![docker pull mysql](/posts/development-cource/docker-pull-mysql.png)

1. pull한 mysql 이미지를 확인한다.

    ```powershell
    docker images # docker image 출력
    ```

    ![docker images](/posts/development-cource/docker-images.png)

1. mysql 컨테이너를 생성 및 실행한다.

    ```powershell
    docker run --name <컨테이너 이름> -e MYSQL_ROOT_PASSWORD=<생성할 mysql 관리자 패스워드> -d -p 3306:3306 <이미지 이름>
    ```

    ![docker run](/posts/development-cource/docker-run.png)

1. 컨테이너를 확인한다.

    ```powershell
    docker ps # docker 컨테이너 출력
    ```

    ![docker pd](/posts/development-cource/docker-ps.png)

1. Docker 컨테이너의 mydql 접속하기

    ```powershell
    # Docker 컨테이너 접속
    docker exec -it <컨테이너 이름> bash

    # Docker 컨테이너 접속 후 mysql 접속
    mysql -u root -p # password를 요구하면 컨테이너 생성 시 설정한 mysql 관리자 패스워드를 입력한다.

    # mysql 접속 후 동작을 확인한다.
    show databases; # 현재 database 목록을 출력한다.

    # mysql 접속 종료
    exit

    # Docker 컨테이너 접속 종료
    exit
    ```

    ![docker exec](/posts/development-cource/docker-exec.png)

1. Docker 컨테이너 시작/중지/재시작/삭제

    ```powershell
    # 컨테이너 시작
    docker start <컨테이너 이름>

    # 컨테이너 중지
    docker stop <컨테이너 이름>

    # 컨테이너 재시작
    docker restart <컨테이너 이름>

    # 컨테이너 삭제(중지 상태에서만 가능)
    docker rm <컨테이너 이름>
    ```
