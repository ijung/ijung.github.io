---
title: 도커를 이용해 MySql 설치 및 실행하기
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
    - [도커 설치]({% post_url 2023-09-09-install-docker %})

1. mysql 이미지를 pull한다.

    ```powershell
    docker pull mysql
    ```

    - 버전을 명시해 주지 않으면 lastest 버전을 받는다.
    - 특정 버전을 받기를 원할 경우 `docker pull mysql:8.0.22`와 같이 실행한다.
    - pull 이미지는 [docker hub](https://hub.docker.com)에서 확인이 가능하다.
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

1. Database 만들기

    ```powershell
    CREATE DATABASE <database 이름> default CHARACTER SET UTF8; 
    ```

    - 데이터 베이스를 생성하고 한글을 사용할 수 있도록 UTF8로 문자열을 저장하게 설정(default CHARACTER SET UTF8)

    ![create database](/posts/development-cource/create-database.png)

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
