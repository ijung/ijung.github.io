---
title: Spring Boot 3.1 Release Notes
author: June
date: 2023-06-24 15:40:00 +0900
categories: [개발 게시물, Spring Boot]
tags: [프레임워크, spring, springboot, spring boot]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Spring Boot 3.1 Release Notes](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.1-Release-Notes)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

### 참고하면 좋을 문서들

## Upgrading from Spring Boot 3.0

### Dependency Management for Apache HttpClient 4

Apache HttpClient 5 사용을 선호하기 위해 `RestTemplate`을 사용하는 Apache HttpClient 4에 대한 지원이 [Spring Framework 6에서는 제거되었습니다](https://github.com/spring-projects/spring-framework/issues/28925). Spring Boot 3.0에는 HttpClient 4와 5 모두에 대한 종속성 관리가 포함되어 있습니다. HttpClient 4를 계속 사용하는 애플리케이션은 [`RestTemplate`를 사용할 때 진단하기 어려운 오류가 발생할 수 있습니다](https://github.com/spring-projects/spring-boot/issues/33515).

Spring Boot 3.1에서는 HttpClient 4에 대한 종속성 관리가 제거되어 사용자가 대신 HttpClient 5로 이동하도록 권장합니다.

### Servlet and Filter Registrations

이제 `ServletRistrationBean` 및 `FilterRistrationBean` 클래스는 등록이 실패할 경우 경고를 기록하는 대신 `IllegalStateException`을 발생시켜 실패합니다. 이전 동작이 필요한 경우 등록 빈에서 `setIgnoreRistrationFailure(true)`를 호출해야 합니다.

### Git Commit ID Maven Plugin Version Property

`io .github.git-commit-id:git-commit-id-maven-plugin`의 버전을 재정의하는 데 사용되는 속성이 해당 아티팩트 이름과 일치하도록 업데이트되었습니다. 이 변경 사항을 적용하려면 `pom.xml`에서 `git-commit-id-plugin.version`을 `git-commit-id-maven-plugin.version`으로 바꾸세요.

### Spring Kafka Retry Topic Auto-configuration

자동 구성된 재시도 가능 토픽 구성(`spring.kafka.retry.topic.enabled: true`)과 함께 Apache Kafka를 사용하는 경우, `maxDelay`을 가진 지수 백오프를 사용하면 이제 `maxDelay` 수준의 모든 재시도가 동일한 토픽으로 전송됩니다. 이전에는 최대 지연(max delay)이 초과되더라도 각 재시도에 대해 별도의 토픽이 사용되었습니다.

예를 들어 최대 재시도 시도가 `5`, 지연이 `1초`, 승수가 `2`, 최대 지연이 `3초`인 경우 초기 실패 후 1초, 2초, 3초, 3초에 재시도가 수행됩니다. 이전 버전의 Spring Boot에서는 프레임워크가 `someTopic`, `someTopic-retry-0`, `someTopic-retry-1`, `someTopic-retry-2`, `someTopic-retry-3`, `someTopic-dlt`의 6개의 토픽을 생성합니다. 이 변경 사항으로 인해 `someTopic-retry-3` 토픽은 만들어지지 않고 대신 3초 재시도 모두 `someTopic-retry-2`에 포함됩니다. 이전 Spring Boot 버전에서 마이그레이션한 후 모든 레코드가 소비된 후 `someTopic-retry-3` 토픽을 안전하게 삭제할 수 있습니다.

### Dependency Management for Testcontainers

이제 Spring Boot의 종속성 관리에는 테스트 컨테이너가 포함됩니다. 필요한 경우, Spring Boot에서 관리하는 버전은 `testcontainers.version` 속성을 사용하여 재정의할 수 있습니다.

### Hibernate 6.2

Spring Boot 3.1에서는 Hibernate가 6.2로 업그레이드됩니다. 이 업그레이드가 애플리케이션에 어떤 영향을 미칠 수 있는지 알아보려면 [Hibernate 6.2 마이그레이션 가이드](https://docs.jboss.org/hibernate/orm/6.2/migration-guide/migration-guide.html)를 참조하세요.

### Jackson 2.15

Spring Boot 3.1에서는 Jackson이 2.15로 업그레이드됩니다. 이것이 애플리케이션에 어떤 영향을 미칠 수 있는지 알아보려면 [Jackson 위키](https://github.com/FasterXML/jackson/wiki/Jackson-Release-2.15)를 참조하세요.

2.15에서 주목할 만한 변경 사항 중 하나는 처리 제한이 도입되었다는 점입니다. 이러한 제약 조건을 조정하려면 다음과 유사하게 `Jackson2ObjectMapperBuilderCustomizer`를 정의하면 됩니다:

```java
@Bean
Jackson2ObjectMapperBuilderCustomizer customStreamReadConstraints() {
  return (builder) -> builder.postConfigurer((objectMapper) -> objectMapper.getFactory().setStreamReadConstraints(StreamReadConstraints.builder().maxNestingDepth(2000).build()));
}
```

### Mockito 5

Spring Boot 3.1에서는 Mockito가 5, 특히 5.3으로 업그레이드됩니다. Mockito 5.x 라인의 주목할 만한 변경 사항에 대해 알아보려면 [Mockito 릴리스 노트](https://github.com/mockito/mockito/releases/tag/v5.0.0)를 참조하세요.

### Health Group Membership Validation

이제 시작 시 상태 그룹의 구성된 멤버십이 유효성을 검사합니다. 존재하지 않는 상태 표시기가 포함되거나 제외된 경우 시작이 실패합니다. 이 유효성 검사를 비활성화하여 이전 버전의 동작으로 복원하려면 `management.endpoint.health.validate-group-membership`을 `false`로 설정하면 됩니다.

### Minimum Requirements Changes

없음.

## New and Noteworthy

| Tip | 구성 변경 사항에 대한 전체 개요는 [구성 변경 로그](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.1.0-Configuration-Changelog)를 확인하세요. |
|:--:|--|

### Service Connections

새로운 서비스 연결 개념(service connection concept)이 도입되었습니다. 이러한 연결은 애플리케이션에서 `ConnectionDetails` 빈으로 표시됩니다. 이러한 빈은 제거 서비스에 대한 연결을 설정하는 데 필요한 세부 정보를 제공하며 Spring Boot의 자동 구성(auto-configuration)이 `ConnectionDetails` 빈을 사용하도록 업데이트되었습니다. 이러한 빈을 사용할 수 있는 경우 연결 관련 구성 속성보다 우선합니다. 연결 풀의 크기 및 동작을 제어하는 속성 등 연결 자체와 관련이 없는 구성 속성은 계속 사용됩니다.

이 하위 수준 기능은 `ConnectionDetails` 빈을 정의하여 서비스 연결을 자동으로 구성하는 다른 상위 수준 기능을 위한 빌딩 블록으로 사용됩니다.

#### Property-based ConnectionDetails Beans

적절한 `...ConnectionDetails` 빈이 다른 곳에 정의되어 있지 않은 경우, 관련 구성 속성에 의해 뒷받침되는 자체 기반을 정의하도록 Spring Boot의 자동 구성이 업데이트되었습니다. 따라서 해당 빈을 사용할 수 없고 속성 기반 구성에 대한 폴백이 필요한 경우를 처리할 필요 없이 `...ConnectionDetails`를 주입할 수 있습니다.

### Testcontainers

#### Using Testcontainers at Development Time

개발 시점에 테스트 컨테이너들(Testcontainers)을 사용하여 외부 서비스를 관리할 수 있는 기능이 도입되었습니다.

[개발 시 테스트 컨테이너들](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.testing.testcontainers.at-development-time)를 사용할 때 테스트 주 메서드를 통해 애플리케이션을 실행하는 데 새로운 Maven 목표(`spring-boot:test-run`)와 Gradle 작업(`bootTestRun`)을 사용할 수 있습니다.

테스트 컨테이너들 `컨테이너(Container)` 인스턴스를 정적 필드로 선언하는 클래스는 새로운 `@ImportTestcontainers` 어노테이션을 사용하여 임포트할 수 있습니다. 자세한 내용은 [참조 문서](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.testing.testcontainers.at-development-time.importing-container-declarations)를 참조하세요.

테스트 컨테이너들 수명 주기 관리가 개선되어 컨테이너가 먼저 초기화되고 마지막으로 파기되도록 합니다. 재사용 가능한 컨테이너에 대한 지원도 개선되었습니다.

이제 `컨테이너` `@Bean` 메서드에서 프로퍼티를 기여하기 위해 `DynamicPropertyRegistry`를 주입할 수 있습니다. 이는 테스트에서 사용할 수 있는 `@DynamicPropertySource`와 유사한 방식으로 작동합니다. 자세한 내용은 [참조 문서](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.testing.testcontainers.at-development-time.dynamic-properties)를 참조하세요.

#### Testcontainers Service Connections

테스트 컨테이너를 사용할 때 일반적으로 컨테이너의 설정을 기반으로 애플리케이션 속성을 구성하기 위해 `@DynamicPropertySource`가 사용됩니다:

```java
@Container
static GenericContainer redis = new GenericContainer(DockerImageName.parse("redis").withTag("4.0.14"));

// …

@DynamicPropertySource
static void redisProperties(DynamicPropertyRegistry registry) {
  registry.add("spring.data.redis.host", redis::getHost);
  registry.add("spring.data.redis.port", redis::getFirstMappedPort);
}
```

이제 다음과 같이 단순화할 수 있습니다.

```java
@Container
@ServiceConnection
static GenericContainer redis = new GenericContainer(DockerImageName.parse("redis").withTag("4.0.14"));
```

여기서 `@ServiceConnection`은 컨테이너가 Redis 연결 세부 정보의 소스로 사용되어야 함을 나타냅니다. `@ServiceConnection` 어노테이션을 제공하는 `spring-boot-testcontainers` 모듈은 컨테이너에서 이러한 세부 정보를 추출하는 동시에 테스트 컨테이너 API를 사용하여 컨테이너를 정의하고 구성할 수 있도록 합니다.

현재 `@ServiceConnection` 어노테이션에서 지원되는 서비스의 전체 목록은 [참조 문서](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.testing.testcontainers.service-connections)를 참조 하세요.

### Docker Compose

새로운 모듈인 `spring-boot-docker-compose`는 Docker Compose와의 통합을 제공합니다. 앱이 시작될 때 Docker Compose 통합은 현재 작업 디렉터리에서 구성 파일을 찾습니다. 지원되는 파일은 다음과 같습니다:

- `compose.yaml`

- `compose.yml`

- `docker-compose.yaml`

- `docker-compose.yml`

비표준 파일을 사용하려면 `spring.docker.compose.file` 속성을 설정하세요.

기본적으로 구성 파일에 선언된 서비스는 `docker compose up`을 사용하여 시작되고 해당 서비스에 대한 연결 세부 정보 빈이 애플리케이션 컨텍스트에 추가되어 추가 구성 없이 서비스를 사용할 수 있습니다. 애플리케이션이 중지되면 docker compose down을 사용하여 서비스가 종료됩니다. 이 수명 주기 관리와 서비스 시작 및 종료에 사용되는 명령은 `spring.docker.compose.lifecycle-management`, `spring.docker.compose .startup.command` 및 `spring.docker.compose.shutdown.command` 구성 속성을 사용하여 사용자 정의할 수 있습니다.

현재 지원되는 서비스 목록을 포함한 자세한 내용은 [참조 문서](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.docker-compose)를 참조 하세요.

### SSL Configuration

이제 Java 키스토어 및 PEM 인코딩 인증서와 같은 SSL 신뢰 자료를 프로퍼티를 사용하여 구성하고 임베디드 웹 서버, 데이터 서비스, `RestTemplate` 및 `WebClient`와 같은 다양한 유형의 연결에 보다 일관된 방식으로 적용할 수 있습니다.

자세한 내용은 [참조 문서](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/features.html#features.ssl)를 참조 하세요.

### Auto-configuration for Spring Authorization Server

이번 릴리스에서는 새로운 `spring-boot-starter-oauth2-authorization-server` 스타터와 함께 [스프링 권한 부여 서버(the Spring Authorization Server)](https://spring.io/projects/spring-authorization-server) 프로젝트에 대한 지원이 제공됩니다. 자세한 내용은 [Spring Boot 참조 문서의 권한 부여 서버 섹션](https://docs.spring.io/spring-boot/docs/3.1.0/reference/html/web.html#web.security.oauth2.authorization-server)에서 확인할 수 있습니다.

### Docker Image Building

#### Image Created Date and Time

#### Image Application Directory

### Spring for GraphQL

#### Exception Handling

#### Pagination and Sorting

#### Improved Schema Type Generation

### Support for Exporting Traces Using OTLP

### Wavefront Span Tag Customization

### Different log levels for file and console

### Maximum HTTP Response Header Size

### ActiveMQ Support

### Dependency Upgrades

### Miscellaneous

## Deprecations in Spring Boot 3.1.0
