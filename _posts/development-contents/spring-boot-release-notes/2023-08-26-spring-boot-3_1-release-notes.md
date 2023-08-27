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

이제 `spring-boot:build-image` Maven 목표 및 `bootBuildImage` Gradle 작업에 생성된 이미지의 메타데이터에서 `Created` 필드 값을 사용자가 지정한 날짜로 설정하거나 현재 날짜와 시간을 사용하도록 `now`로 설정하는 데 사용할 수 있는 `createdDate` 구성 옵션이 있습니다. 자세한 내용은 [Gradle](https://docs.spring.io/spring-boot/docs/3.1.0/gradle-plugin/reference/htmlsingle/#build-image.customization) 및 [Maven](https://docs.spring.io/spring-boot/docs/3.1.0/maven-plugin/reference/htmlsingle/#build-image.customization) 플러그인 설명서를 참조하세요.

#### Image Application Directory

이제 `spring-boot:build-image` Maven 목표 및 `bootBuildImage` Gradle 작업에 빌드팩이 사용할 애플리케이션 콘텐츠를 업로드할 빌더 이미지의 위치를 설정하는 데 사용할 수 있는 `applicationDirectory` 구성 옵션이 있습니다. 이 위치는 생성된 이미지에서 애플리케이션 콘텐츠의 위치이기도 합니다. 자세한 내용은 [Gradle](https://docs.spring.io/spring-boot/docs/3.1.0/gradle-plugin/reference/htmlsingle/#build-image.customization) 및 [Maven](https://docs.spring.io/spring-boot/docs/3.1.0/maven-plugin/reference/htmlsingle/#build-image.customization) 플러그인 설명서를 참조하세요.

### Spring for GraphQL

#### Exception Handling

컨트롤러 또는 `@ControllerAdvice`에 선언된 `@GraphQlExceptionHandler` 메서드는 이제 컨트롤러 메서드 호출을 위해 Spring for GraphQL에서 기본적으로 지원됩니다. 또한 Spring Boot는 `GraphQlSource` 구성을 통해 `QueryDslDataFetcher`, `QueryByExampleDataFetcher` 등과 같은 기타(컨트롤러가 아닌) `DataFetcher` 구현에 대한 `@ControllerAdvice` 예외 처리를 자동 구성합니다.

#### Pagination and Sorting

Spring 데이터가 클래스 경로에 있는 경우, 이제 GraphQL용 Spring이 페이지 매김 및 정렬을 지원하도록 자동 구성됩니다.

#### Improved Schema Type Generation

`GraphQlSource`는 이제 `ConnectionTypeDefinitionConfigurer`로 자동 구성됩니다. [GraphQL 커서 연결 사양](https://relay.dev/graphql/connections.htm)에서 `연결 유형(Connection Type)`으로 간주되는 유형 정의 이름이 "Connection"으로 끝나는 필드를 찾아서 "연결(Connection)" 유형을 생성하고, 필요한 유형 정의가 아직 없는 경우 추가합니다.

### Support for Exporting Traces Using OTLP

클래스 경로에 `io .opentelemetry:opentelemetry-exporter-otlp`가 있으면 `OtlpHttpSpanExporter`가 자동으로 구성됩니다. 내보내기 구성은 `management.otlp.tracing.*` 구성 속성을 사용하여 사용자 지정할 수 있습니다.

### Wavefront Span Tag Customization

Wavefront를 사용 중이고 R[ED 지표에 대한 스팬 태그를 사용자 지정](https://docs.wavefront.com/tracing_customize_spans_and_alerts.html)하려는 경우, 이제 이 작업을 수행할 수 있는 `management.wavefront.trace-derived-custom-tag-keys`라는 새로운 속성이 있습니다. 자세한 내용은 [#34194](https://github.com/spring-projects/spring-boot/issues/34194)를 참조하세요.

### Different log levels for file and console

Logback 또는 Log4j2를 사용하는 경우 이제 콘솔 로그와 파일 로그에 대해 서로 다른 로그 수준을 설정할 수 있는 옵션이 있습니다. 이 옵션은 구성 속성 `logging.threshold.console` 및 `logging.threshold.file`을 사용하여 설정할 수 있습니다.

### Maximum HTTP Response Header Size

이제 Tomcat 또는 Jetty를 사용하는 경우 최대 HTTP 응답 헤더 크기를 제한할 수 있습니다. Tomcat의 경우 `server.tomcat. max-http-response-header-size` 속성을, Jetty의 경우 `server.jetty.max-http-response-header-size` 속성을 사용할 수 있습니다. 기본적으로 응답 헤더는 `8kb`로 제한되어 있습니다.

### ActiveMQ Support

Spring Boot 3.0에서 제거되었던 ActiveMQ 클라이언트의 자동 구성에 대한 지원이 복원되었습니다. 임베디드 ActiveMQ 브로커에 대한 지원은 ActiveMQ의 브로커가 아직 JMS 3.0을 지원하지 않으므로 복원되지 않았습니다.

### Dependency Upgrades

Spring Boot 3.1.0은 여러 Spring 프로젝트의 새 버전으로 이동합니다:

- [Spring Authorization Server 1.1.0](https://github.com/spring-projects/spring-authorization-server/releases/tag/1.1.0)
- [Spring Batch 5.0.2](https://github.com/spring-projects/spring-batch/releases/tag/v5.0.2)
- [Spring Data 2023.0.0](https://github.com/spring-projects/spring-data-commons/wiki/Spring-Data-2023.0-%28Ullman%29-Release-Notes)
- [Spring Framework 6.0.9](https://github.com/spring-projects/spring-framework/releases/tag/v6.0.9)
- [Spring GraphQL 1.2.0](https://github.com/spring-projects/spring-graphql/releases/tag/v1.2.0)
- [Spring HATEOAS 2.1.0](https://github.com/spring-projects/spring-hateoas/releases/tag/2.1.0)
- [Spring Integration 6.1.0](https://github.com/spring-projects/spring-integration/releases/tag/v6.1.0)
- [Spring Kafka 3.0.7](https://github.com/spring-projects/spring-kafka/releases/tag/v3.0.7)
- [Spring LDAP 3.1.0](https://github.com/spring-projects/spring-ldap/releases/tag/3.1.0-RC1)
- [Spring Security 6.1.0](https://github.com/spring-projects/spring-security/releases/tag/6.1.0)
- [Spring Session 3.1.0](https://github.com/spring-projects/spring-session/releases/tag/3.1.0)
- [Spring Web Services 4.0.4](https://github.com/spring-projects/spring-ws/releases/tag/v4.0.4)

수많은 타사 종속성도 업데이트되었는데, 그 중 주목할 만한 몇 가지를 소개하면 다음과 같습니다:

- [Couchbase Java Client 3.4.6](https://docs.couchbase.com/java-sdk/current/project-docs/sdk-release-notes.html#version-3-4-6-4-may-2023)
- [Elasticsearch Client 8.7](https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/release-highlights.html#_version_8_7)
- [Hibernate 6.2](https://in.relation.to/2023/03/30/orm-62-final/)
- [GraphQL Java 20.1](https://github.com/graphql-java/graphql-java/releases/tag/v20.1)
- [Jackson 2.15.0](https://github.com/FasterXML/jackson/wiki/Jackson-Release-2.15#changes-compatibility)
- [Kafka 3.4.0](https://downloads.apache.org/kafka/3.4.0/RELEASE_NOTES.html)
- [Kotlin 1.8.21](https://github.com/JetBrains/kotlin/releases/tag/v1.8.21)
- [Liquibase 4.20](https://forum.liquibase.org/t/liquibase-4-20-released/7874)
- [Micrometer 1.11.0](https://github.com/micrometer-metrics/micrometer/releases/tag/v1.11.0)
- [Micrometer Tracing 1.1.1](https://github.com/micrometer-metrics/tracing/releases/tag/v1.1.1)
- [Mockito 5.3](https://github.com/mockito/mockito/releases/tag/v5.3.0)
- [Native Build Tools 0.9.22](https://github.com/graalvm/native-build-tools/releases/tag/0.9.22)
- [Neo4j Java Driver 5.8.0](https://github.com/neo4j/neo4j-java-driver/releases/tag/5.8.0)
- [OpenTelemetry 1.24.0](https://github.com/open-telemetry/opentelemetry-java/releases/tag/v1.24.0)
- [Rabbit AMQP Client 5.17.0](https://github.com/rabbitmq/rabbitmq-java-client/releases/tag/v5.17.0)
- [Reactor BOM 2022.0.7](https://github.com/reactor/reactor/releases/tag/2022.0.7)
- [Testcontainers 1.18](https://github.com/testcontainers/testcontainers-java/releases/tag/1.18.0)
- Undertow 2.3.6.Final

### Miscellaneous

위에 나열된 변경 사항 외에도 다음과 같은 많은 사소한 조정과 개선이 이루어졌습니다:

- 스프링 카프카 `ContainerCustomizer` 빈이 이제 자동 구성된 `KafkaListenerContainerFactory`에 적용됩니다.
- OTLP 레지스트리로 헤더 전송을 지원하기 위해 `management.otlp.metrics.export.headers` 프로퍼티가 추가되었습니다.
- 이제 `JoranConfigurators` 빈을 AOT 처리에 사용할 수 있습니다.
- `spring.kafka.admin`에 `close-timeout`, `operation-timeout`, `auto-startup` 및 `auto-create` 프로퍼티가 추가되었습니다.
- 이제 자동 구성된 `ConcurrentKafkaListenerContainerFactory`에 `BatchInterceptor` 빈이 적용됩니다.
- 인식되는 `CloudPlaform` 값 목록에 Nomad가 추가되었습니다.
- 이제 `spring.jmx`에 대한 `registration-policy` 속성을 지정할 수 있습니다.
- `SanitizableData`에 `withSanitizedValue` 유틸리티 메서드가 추가되었습니다.
- `RabbitTemplateCustomizer`가 도입되었습니다. 이 유형의 빈은 자동 구성된 `RabbitTemplate`를 사용자 정의합니다.
- CNB Platform API 0.11이 지원됩니다.
- `spring-boot-starter-parent`는 `maven.compiler.release`를 구성된 Java 버전으로 설정합니다.
- 이제 `-Dspring-boot.build-info.skip`을 설정하여 `build-info` 목표를 건너뛸 수 있습니다.
- 마이크로미터의 `OtlpMeterRegistry`에 대한 집계 임시성 구성 지원.
- Log4j2 및 Logback에서 추가 색상 지원.
- R2DBC MySQL 드라이버(`io.asyncer:r2dbc-mysql`)에 대한 종속성 관리가 추가되었습니다.
- R2DBC MariaDB 드라이버(`org.mariadb:r2dbc-mariadb`)에 대한 종속성 관리가 추가되었습니다.
- OpenTelemetry 사용 시 자동 구성된 `SdkTracerProvider`를 생성하는 데 사용되는 `SdkTracerProviderBuilder`를 `SdkTracerProviderBuilderCustomizer` 빈을 정의하여 사용자 지정할 수 있습니다.
- `MockServerRestTemplateCustomizer`는 이제 새로운 `setBufferContent` 메서드를 통해 콘텐츠 버퍼링 활성화를 지원합니다.
- 스프링 배치가 자동 구성될 때 사용하는 변환 서비스는 이제 `BatchConversionServiceCustomizer` 빈을 정의하여 사용자 지정할 수 있습니다.
- JWK Set URI에 대한 JTW 디코더를 생성하는 데 사용되는 빌더는 `JwkSetUriReactiveJwtDecoderBuilderCustomizer` 또는 `JwkSetUriJwtDecoderBuilderCustomizer` 빈을 정의하여 커스터마이즈할 수 있습니다.
- `io.r2dbc:r2dbc-mssql`에 대한 종속성 관리가 복원되었습니다.
- 로그백의 루트 로그 레벨이 가능한 한 빨리 `INFO`로 기본 설정됩니다.
- 기본적으로 Docker Compose는 이제 `down`이 아닌 `stop`을 사용하여 중지됩니다.

## Deprecations in Spring Boot 3.1.0

- `spring.kafka.streams.cache-max-size-buffering`은 `spring.kafka.streams.state-store-cache-max-size`를 위해 더 이상 사용되지 않습니다.
- `MongoPropertiesClientSettingsBuilderCustomizer`를 `StandardMongoClientSettingsBuilderCustomizer`로 변경합니다.
- `org.springframework.boot.autoconfigure.security.oauth2.client.OAuth2ClientPropertiesRegistrationAdapter`를 `org.springframework.boot.autoconfigure.security.oauth2.client.OAuth2ClientPropertiesMapper`로 변경합니다.
- SSL 번들을 위해 `org.springframework.boot.web.server.SslStoreProvider`가 더 이상 사용되지 않습니다.
