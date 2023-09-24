---
title: Spring Boot Admin Docs - Getting started(3.1.6)
author: June
date: 2023-09-03 12:57:00 +0900
categories: [개발 게시물, Spring Boot]
tags: [프레임워크, spring, springboot, spring boot]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Spring Boot Admin Docs - Getting started(3.1.6)](https://docs.spring-boot-admin.com/current/getting-started.html)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

## Overview

Spring Boot Admin은 스프링 부트 액추에이터(Spring Boot Actuators)가 제공하는 정보를 보기 좋고 접근하기 쉬운 방식으로 시각화하는 것을 목표로 하는 모니터링 도구입니다. 크게 두 부분으로 구성됩니다:

- 스프링 부트 액추에이터를 표시하고 상호 작용할 수 있는 사용자 인터페이스를 제공하는 `서버(server)`입니다.

- 서버에 등록하고 액추에이터 엔드포인트에 액세스할 수 있도록 허용하는 데 사용되는 `클라이언트(client)`입니다.

## Quick Start

Spring Boot Admin은 Spring Boot에 의존하므로 먼저 Spring Boot 애플리케이션을 설정해야 합니다. [https://start.spring.io](https://start.spring.io)을 사용하여 이 작업을 수행하는 것이 좋습니다. Spring Boot 관리 서버는 서블릿 또는 웹플럭스 애플리케이션으로 실행할 수 있으므로 이를 결정하고 해당 Spring Boot 스타터를 추가해야 합니다. 이 예에서는 서블릿 웹 스타터를 사용합니다.

1. 종속 요소에 Spring Boot Admin Server starter를 추가하세요:

    ```xml
    <dependency>
        <groupId>de.codecentric</groupId>
        <artifactId>spring-boot-admin-starter-server</artifactId>
        <version>3.1.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    > ## Compatibility Matrix
    >
    > In the Spring Boot Admin Server App, the Spring Boot Admin's version matches the major and minor versions of Spring Boot.
    > Spring Boot Admin Server 앱에서 Spring Boot Admin의 버전은 Spring Boot의 메이저 버전 및 마이너 버전과 일치합니다.
    >
    > | Spring Boot Version | Spring Boot Admin  |
    > |---------------------|--------------------|
    > | 2.6                 | 2.6.Y              |
    > | 2.7                 | 2.7.Y              |
    > | 3.0                 | 3.0.Y              |
    >
    > 그럼에도 불구하고 서비스의 기본 Spring Boot 버전과 관계없이 모든 버전의 Spring Boot 서비스를 모니터링할 수 있습니다.
    > 따라서 Spring Boot Admin Server 버전 2.6을 실행하고 Spring Boot Admin Client 버전 2.3을 사용하여 Spring Boot 2.3에서 실행되는 서비스를 모니터링하는 것이 가능합니다.

1. 구성에 `@EnableAdminServer`를 추가하여 Spring Boot Admin Server configuration을 가져옵니다:

    ```java
    @Configuration
    @EnableAutoConfiguration
    @EnableAdminServer
    public class SpringBootAdminApplication {
        public static void main(String[] args) {
            SpringApplication.run(SpringBootAdminApplication.class, args);
        }
    }
    ```

    서블릿 컨테이너에서 war-deployment를 통해 Spring Boot Admin Server를 설정하려면 [spring-boot-admin-sample-war](https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-war/)를 살펴보시기 바랍니다.

    또한 보안을 추가하는 [spring-boot-admin-sample-servlet](https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-servlet/)프로젝트도 참조하세요.

## Registering Client Applications

SBA Server에 애플리케이션을 등록하려면 SBA Client를 포함하거나 [Spring Cloud Discovery](https://spring.io/projects/spring-cloud)(예: Eureka, Consul, ...)를 사용할 수 있습니다. [SBA Server side에서 정적 구성을 사용하는 간단한 옵션도 있습니다](https://docs.spring-boot-admin.com/current/server.html#spring-cloud-discovery-static-config).

### Spring Boot Admin Client

등록하려는 각 애플리케이션에는 Spring Boot Admin Client가 포함되어야 합니다. 엔드포인트를 보호하려면 `spring-boot-starter-security`도 추가하세요.

1. spring-boot-admin-starter-client를 종속성에 추가하세요:

    ```xml
    <dependency>
        <groupId>de.codecentric</groupId>
        <artifactId>spring-boot-admin-starter-client</artifactId>
        <version>3.1.6</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    ```

1. Spring Boot Admin Server의 URL을 구성하여 SBA 클라이언트를 사용하도록 설정합니다:

    ```properties
    spring.boot.admin.client.url=http://localhost:8080  (1)
    management.endpoints.web.exposure.include=*  (2)
    management.info.env.enabled=true (3)
    ```

    (1) 등록할 Spring Boot Admin Server의 URL입니다.

    (2) Spring Boot 2와 마찬가지로 대부분의 엔드포인트는 기본적으로 http를 통해 노출되지 않습니다. 여기서는 와일드 카드(*) 옵션을 통해 모든 엔드포인트를 노출합니다. 프로덕션 환경에서는 어떤 엔드포인트를 노출할지 신중하게 선택해야 합니다.

    (3) Spring Boot 2.6부터 env info contributor는 기본적으로 비활성화되어 있습니다. 따라서 이를 활성화해야 합니다.

1. 액추에이터 엔드포인트에 액세스할 수 있도록 설정합니다:

    ```java
    @Configuration
    public static class SecurityPermitAllConfig {
        @Bean
        protected SecurityFilterChain filterChain(HttpSecurity http) {
            return http.authorizeHttpRequests((authorizeRequests) -> authorizeRequests.anyRequest().permitAll())
                .csrf().disable().build();
        }
    }
    ```

    1 간결함을 위해 지금은 보안을 비활성화합니다. 보안 엔드포인트를 처리하는 방법은 [보안 섹션](https://docs.spring-boot-admin.com/current/security.html#securing-spring-boot-admin)을 참조하세요.

### Spring Cloud Discovery

애플리케이션에 이미 Spring Cloud Discovery를 사용하고 있다면 SBA Client가 필요하지 않습니다. Spring Boot Admin Server에 DiscoveryClient를 추가하기만 하면 나머지는 AutoConfiguration에 의해 수행됩니다.

다음 단계에서는 Eureka를 사용하지만 다른 Spring Cloud Discovery 구현도 지원됩니다. [Consul](https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-consul/)과 [Zookeeper](https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-zookeeper/)를 사용하는 예제는 링크를 참고하세요.

또한 [Spring Cloud documentation](https://spring.io/projects/spring-cloud)를 참조하세요.

1. 스프링 클라우드 스타터 유레카를 종속 요소에 추가하세요:

    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    ```

1. 구성에 `@EnableDiscoveryClient`를 추가하여 검색을 사용하도록 설정하세요:

    ```java
    @Configuration
    @EnableAutoConfiguration
    @EnableDiscoveryClient
    @EnableScheduling
    @EnableAdminServer
    public class SpringBootAdminApplication {
        public static void main(String[] args) {
            SpringApplication.run(SpringBootAdminApplication.class, args);
        }
    }
    ```

1. 유레카 클라이언트에게 서비스 레지스트리를 찾을 수 있는 위치를 알려줍니다:

    ```properties
    eureka:   (1)
        instance:
            leaseRenewalIntervalInSeconds: 10
            health-check-url-path: /actuator/health
            metadata-map:
            startup: ${random.int}    #needed to trigger info and endpoint update after restart
        client:
            registryFetchIntervalSeconds: 5
            serviceUrl:
            defaultZone: ${EUREKA_SERVICE_URL:http://localhost:8761}/eureka/

        management:
        endpoints:
            web:
            exposure:
                include: "*"  (2)
        endpoint:
            health:
            show-details: ALWAYS
    ```

    (1) Eureka 클라이언트의 구성 섹션

    (2) Spring Boot 2와 마찬가지로 대부분의 엔드포인트는 기본적으로 http를 통해 노출되지 않습니다. 여기서는 와일드 카드(*) 옵션을 통해 모든 엔드포인트를 노출합니다. 프로덕션 환경에서는 어떤 엔드포인트를 노출할지 신중하게 선택해야 합니다.

[spring-boot-admin-sample-eureka](https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-eureka/)도 참조하세요.
  
> Spring Boot Admin Server를 유레카 서버에 포함할 수 있습니다. 위에서 설명한 대로 모든 것을 설정하고 `spring.boot.admin.context-path`를 `"/ "`가 아닌 다른 것으로 설정하여 Spring Boot 관리자 서버 UI가 Eureka의 UI와 충돌하지 않도록 합니다

## Use of SNAPSHOT-Versions

Spring Boot Admin Server의 스냅샷 버전을 사용하려면 spring 및 sonatype 스냅샷 리포지토리를 포함해야 할 가능성이 높습니다:

```xml
<repositories>
    <repository>
        <id>spring-milestone</id>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
        <url>http://repo.spring.io/milestone</url>
    </repository>
    <repository>
        <id>spring-snapshot</id>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <url>http://repo.spring.io/snapshot</url>
    </repository>
    <repository>
        <id>sonatype-nexus-snapshots</id>
        <name>Sonatype Nexus Snapshots</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>false</enabled>
        </releases>
    </repository>
</repositories>
```
