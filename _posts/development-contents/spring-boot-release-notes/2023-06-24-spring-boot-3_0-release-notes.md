---
title: Spring Boot 3.0 Release Notes
author: June
date: 2023-06-24 14:00:00 +0900
categories: [개발 게시물, Spring Boot]
tags: [프레임워크, spring, springboot, spring boot]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Spring Boot 3.0 Release Notes](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Release-Notes)를 번역 및 이해를 위한 일부 내용을 덧 붙인 글입니다.

### 참고하면 좋을 문서들

- [docs](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html)
- [actuator-api-docs](https://docs.spring.io/spring-boot/docs/3.0.0/actuator-api/htmlsingle)
- [gradle-plugin-docs](https://docs.spring.io/spring-boot/docs/3.0.0/gradle-plugin/htmlsingle)

## Upgrading from Spring Boot 2.7

스피링 부트 3.0은 스프링 부트의 주요 릴리즈(major release)로, 기존 애플리케이션을 업그레이드하는 것이 평소보다 조금 더 복잡할 수 있습니다.
우리는 기존의 스프링 부트 2.7 애플리케이션을 업그레이드하는 데 도움이 될 수 있는 [전용 마이그레이션 가이드](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide)를 준비했습니다.

현재 스프링 부트의 이전 버전을 사용 중이라면, 스프링 부트 3.0으로 이전하기 전에 반드시 [스프링 부트 2.7로 업그레이드](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.7-Release-Notes)하시는 것을 강력히 권장합니다.

## New and Noteworthy

|Tip|설정 변경에 대한 전체 개요는 [설정 변경 로그](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Configuration-Changelog)에서 확인하세요.|
|----|----|

### Java 17 Baseline and Java 19 Support

Spring Boot 3.0은 최소한 Java 17 버전이 필요합니다. 현재 Java 8 또는 Java 11을 사용하고 있다면, Spring Boot 3.0 애플리케이션을 개발하기 전에 JDK를 업그레이드해야 합니다.

Spring Boot 3.0은 JDK 19와 잘 작동하며, JDK 19에서 테스트되었습니다.

#### GraalVM Baseline and Native Build Tools

Spring Boot는 Graal 22.3 이상 및 Native Build Tools Plugin 0.9.17 이상이 필요합니다.

### Third-party Library Upgrades

Spring Boot 3.0은 Spring Framework 6을 기반으로 하며 이를 필요로 합니다. [Spring Framework 6.0에서 사용 가능한 새로운 기능](https://github.com/spring-projects/spring-framework/wiki/What's-New-in-Spring-Framework-6.x)에 대해 읽어보시면 좋을 것입니다.

이번 릴리즈에서 업그레이드된 다른 Spring 프로젝트는 다음과 같습니다:

- [Spring AMQP
  3.0](https://github.com/spring-projects/spring-amqp/releases/tag/v3.0.0).

- [Spring Batch
  5.0](https://github.com/spring-projects/spring-batch/releases/tag/v5.0.0).

- [Spring Data
  2022.0](https://github.com/spring-projects/spring-data-commons/wiki/Spring-Data-2022.0-%28Turing%29-Release-Notes).

- [Spring GraphQL
  1.1](https://github.com/spring-projects/spring-graphql/releases/tag/v1.1.0).

- [Spring HATEOAS
  2.0](https://github.com/spring-projects/spring-hateoas/releases/tag/2.0.0).

- [Spring Integration
  6.0](https://github.com/spring-projects/spring-integration/releases/tag/v6.0.0).

- [Spring Kafka
  3.0](https://github.com/spring-projects/spring-kafka/releases/tag/v3.0.0).

- [Spring LDAP
  3.0](https://github.com/spring-projects/spring-ldap/releases/tag/3.0.0).

- [Spring REST Docs
  3.0](https://github.com/spring-projects/spring-restdocs/wiki/Spring-REST-Docs-3.0-Release-Notes).

- [Spring Retry
  2.0](https://github.com/spring-projects/spring-retry/releases/tag/v2.0.0).

- [Spring Security
  6.0](https://github.com/spring-projects/spring-security/releases/tag/6.0.0)
  (see also [what’s
  new](https://docs.spring.io/spring-security/reference/6.0/whats-new.html)).

- [Spring Session
  3.0](https://github.com/spring-projects/spring-session/releases/tag/3.0.0)

- [Spring WS
  4.0](https://github.com/spring-projects/spring-ws/releases/tag/4.0.0).

Spring Boot 3.0은 모든 의존성에 대해 Java EE에서 Jakarta EE API로 마이그레이션하였습니다. 가능한 한 Jakarta EE 10 호환 의존성이 선택되었습니다, 이에는 다음이 포함됩니다:

- Jakarta Activation 2.1

- Jakarta JMS 3.1

- Jakarta JSON 2.1

- Jakarta JSON Bind 3.0

- Jakarta Mail 2.1

- Jakarta Persistence 3.1

- Jakarta Servlet 6.0

- Jakarta Servlet JSP JSTL 3.0

- Jakarta Transaction 2.0

- Jakarta Validation 3.0

- Jakarta WebSocket 2.1

- Jakarta WS RS 3.1

- Jakarta XML SOAP 3.0

- Jakarta XML WS 4.0

가능한 한 곳에서는 최신 안정화 릴리즈의 서드파티 jar로 업그레이드했습니다. 여기에서 주목할 만한 의존성 업그레이드는 다음과 같습니다:

- Couchbase Client 3.4

- [Ehcache
  3.10](https://github.com/ehcache/ehcache3/releases/tag/v3.10.0)

- Elasticsearch Client 8.5

- [Flyway
  9](https://flywaydb.org/documentation/learnmore/releaseNotes#9.0.0)

- [Groovy 4.0](https://groovy-lang.org/releasenotes/groovy-4.0.html)

- [Hibernate
  6.1](https://in.relation.to/2022/06/24/hibernate-orm-61-features/)

- Hibernate Validator 8.0

- Jackson 2.14

- Jersey 3.1

- Jetty 11

- jOOQ 3.16

- Kotlin 1.7.20

- [Liquibase 4.17](https://docs.liquibase.com/release-notes/home.html)

- [Lettuce
  6.2](https://github.com/lettuce-io/lettuce-core/releases/tag/6.2.0.RELEASE)

- [Log4j
  2.18](https://logging.apache.org/log4j/2.x/changes-report.html#a2.18.0)

- [Logback 1.4](https://logback.qos.ch/news.html)

- [Micrometer
  1.10](https://github.com/micrometer-metrics/micrometer/releases/tag/v1.10.0)

- [Micrometer Tracing
  1.0](https://github.com/micrometer-metrics/tracing/releases/tag/v1.0.0)

- Neo4j Java Driver 5.2

- Netty 4.1.77.Final

- [OkHttp
  4.10](https://square.github.io/okhttp/changelogs/changelog_4x/#version-4100)

- [R2DBC 1.0](https://r2dbc.io/2022/04/25/r2dbc-1.0-goes-ga)

- [Reactor
  2022.0](https://github.com/reactor/reactor/releases/tag/2022.0.0)

- [SLF4J 2.0](https://www.slf4j.org/news.html)

- SnakeYAML 1.32

- Tomcat 10

- Thymeleaf 3.1.0.M2

- Undertow 2.2.20.Final

#### GraalVM Native Image Support

Spring Boot 3.0 애플리케이션은 이제 GraalVM 네이티브 이미지로 변환될 수 있으며, 이는 메모리 및 시작 성능 향상에 상당한 향상을 제공할 수 있습니다. GraalVM 네이티브 이미지를 지원하는 것은 전체 Spring 포트폴리오에 걸쳐 수행된 주요 엔지니어링 노력이었습니다.

GraalVM 네이티브 이미지를 시작하려면, [업데이트된 Spring Boot 참조 문서](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html/native-image.html#native-image)를 확인하십시오.

#### Log4j2 Enhancements

Log4j2 지원이 업데이트되어 다음 기능을 제공하는 새로운 확장 기능을 가지게 되었습니다:

- 프로필 특정 구성

- 환경 속성 조회

- Log4j2 시스템 속성

자세한 내용은 [업데이트된 문서](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html/features.html#features.logging.log4j2-extensions)를 확인하십시오.

#### Improved @ConstructorBinding Detection

생성자 바인딩 `@ConfigurationProperties`을 사용할 때 클래스에 단일 매개변수 생성자가 있다면 `@ConstructorBinding` 어노테이션이 더 이상 필요하지 않습니다.
만약 두 개 이상의 생성자가 있다면, Spring Boot에게 어느 것을 사용할지 알리기 위해 여전히 `@ConstructorBinding`을 사용해야 합니다.

대부분의 사용자에게 이 업데이트된 로직은 더 간단한 `@ConfigurationProperties` 클래스를 가능하게 할 것입니다.
그러나, `@ConfigurationProperties`가 있고 생성자에 빈을 주입하기보다는 바인딩하고 싶다면, 이제 `@Autowired` 어노테이션을 추가해야 합니다.

#### Micrometer Updates

##### Auto-configuration for Micrometer Observation API

Spring Boot 3.0은 Micrometer 1.10에서 도입된 새로운 관찰 API(observation API)를 지원합니다.
새로운 `ObservationRegistry` 인터페이스는 메트릭과 추적 모두에 대한 단일 API를 제공하는 관찰(observations)을 생성하는 데 사용할 수 있습니다.
Spring Boot는 이제 `ObservationRegistry` 인스턴스를 자동으로 구성합니다.

`micrometer-core`가 클래스 경로에 있다면, `DefaultMeterObservationHandler`가 `ObservationRegistry`에 등록되고, 이는 모든 중단된 `Observation`이 타이머로 이어진다는 것을 의미합니다.
`ObservationPredicate`, `GlobalObservationConvention`, 그리고 `ObservationHandler`는 자동으로 `ObservationRegistry`에 등록됩니다.
필요한 경우 `ObservationRegistryCustomizer`를 사용하여 `ObservationRegistry`를 추가로 사용자 지정할 수 있습니다.

자세한 내용은 참조 문서의 [새로운 'Observability' 섹션](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html/actuator.html#actuator.observability)을 참조하십시오.

##### Auto-configuration for Micrometer Tracing

Spring Boot는 이제 [Micrometer Tracing](https://micrometer.io/docs/tracing)을 자동으로 구성합니다.
이에는 Brave, OpenTelemetry, Zipkin, 그리고 Wavefront에 대한 지원이 포함됩니다.

Micrometer Observation API를 사용할 때, 관찰을 마치는 것은 Zipkin이나 Wavefront에 보고되는 스팬으로 이어집니다.
추적은 `management.tracing` 아래의 속성으로 제어할 수 있습니다.
Zipkin은 `management.zipkin.tracing`으로 구성할 수 있으며, Wavefront는 `management.wavefront`를 사용합니다.

추가해야 할 다양한 의존성을 포함한 더 많은 세부 정보는 참조 문서의 [추적 섹션](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html/actuator.html#actuator.micrometer-tracing)에 있습니다.

##### Auto-configuration for Micrometer’s OtlpMeterRegistry

`io.micrometer:micrometer-registry-otlp`이 클래스 경로에 있을 때 `OtlpMeterRegistry`가 이제 자동으로 구성됩니다.
메터 레지스트리는 `management.otlp.metrics.export.*` 속성을 사용하여 구성할 수 있습니다.

#### Prometheus Support

##### Auto-Configuration for Prometheus Exemplars

Micrometer Tracing `Tracer` 빈과 Prometheus가 클래스 경로에 있을 때, `SpanContextSupplier`가 이제 자동으로 구성됩니다.
이 공급자는 현재의 추적 ID와 스팬 ID를 Prometheus에 사용 가능하게 함으로써 메트릭을 추적에 연결합니다.

##### Making a PUT to Prometheus Push Gateway on Shutdown

Push Gateway는 [종료 시 `PUT`을 수행](https://github.com/prometheus/pushgateway#put-method)하도록 구성할 수 있습니다.
이를 수행하려면, `management.prometheus.metrics.export.pushgateway.shutdown-operation`을 `put`으로 설정합니다.
추가로, 기존의 `push` 설정은 더 이상 사용되지 않고 `post`가 대신 사용되어야 합니다.

#### More Flexible Auto-configuration for Spring Data JDBC

Spring Data JDBC에 대한 자동 구성이 이제 더 유연해졌습니다.
Spring Data JDBC에 필요한 여러 자동 구성된 빈들이 이제 조건부이며, 동일한 유형의 빈을 정의함으로써 대체될 수 있습니다. 이제 대체될 수 있는 빈들의 유형은 다음과 같습니다:

- `org.springframework.data.jdbc.core.JdbcAggregateTemplate`

- `org.springframework.data.jdbc.core.convert.DataAccessStrategy`

- `org.springframework.data.jdbc.core.convert.JdbcConverter`

- `org.springframework.data.jdbc.core.convert.JdbcCustomConversions`

- `org.springframework.data.jdbc.core.mapping.JdbcMappingContext`

- `org.springframework.data.relational.RelationalManagedTypes`

- `org.springframework.data.relational.core.dialect.Dialect`

#### Enabling Async Acks with Apache Kafka

Kafka와 함께 비동기 acks를 활성화하기 위한 새로운 구성 속성인 `spring.kafka.listener.async-acks`가 추가되었습니다.
비동기 acks를 활성화하려면, 이 속성을 `true`로 설정하십시오.
이 속성은 `spring.kafka.listener.async-mode`가 `manual` 또는 `manual-immediate`로 설정될 때만 적용됩니다.

#### Elasticsearch Java Client

[새로운 Elasticsearch Java 클라이언트](https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/8.3/index.html)에 대한 자동 구성이 도입되었습니다.
기존의 `spring.elasticsearch.*` 구성 속성을 사용하여 구성할 수 있습니다.

#### Auto-configuration of JdkClientHttpConnector

Reactor Netty, Jetty의 반응형 클라이언트, 그리고 Apache HTTP 클라이언트가 없는 경우, `JdkClientHttpConnector`가 이제 자동으로 구성됩니다.
이를 통해 `WebClient`는 JDK의 `HttpClient`와 함께 사용될 수 있습니다.

#### @SpringBootTest with Main Methods

`@SpringBootTest`와 메인 메소드
`@SpringBootTest` 어노테이션은 이제 사용 가능한 경우 발견된 `@SpringBootConfiguration` 클래스의 `main`을 사용할 수 있습니다.
이는 메인 메소드에 의해 수행된 모든 사용자 정의 `SpringApplication` 구성이 이제 테스트에 의해 선택될 수 있음을 의미합니다.

테스트에 `main` 메소드를 사용하려면, `@SpringBootTest`의 `useMainMethod` 속성을 `UseMainMethod.ALWAYS` 또는 `UseMainMethod.WHEN_AVAILABLE`로 설정하십시오.

자세한 내용은 [업데이트된 참조 문서](https://docs.spring.io/spring-boot/docs/3.0.0/reference/html/features.html#features.testing.spring-boot-applications.using-main)를 참조하십시오.

#### Miscellaneous

위에서 나열된 변경사항 외에도, 약간의 수정과 개선이 있었습니다:

- 호스트 이름은 더 이상 애플리케이션 시작 동안에 로그에 남지 않습니다. 이는 네트워크 검색을 방지하여 시작 시간을 개선하는 데 도움이 됩니다.

- JDK에서 deprecated 처리된 `SecurityManager`에 대한 지원이 제거되었습니다.

- Spring Framework 6에서 제거된 `CommonsMultipartResolver`에 대한 지원이 제거되었습니다.

- Spring Framework의 변화를 따라 `spring.mvc.ignore-default-model-on-redirect`가 deprecated 처리되었습니다.

- WebJars 리소스 핸들러 경로 패턴은 `spring.mvc.webjars-path-pattern` 또는 `spring.webflux.webjars-path-pattern`을 사용하여 커스터마이즈 할 수 있습니다.

- Tomcat의 원격 IP 밸브의 신뢰할 수 있는 프록시는 `server.tomcat.remoteip.trusted-proxies`를 사용하여 구성할 수 있습니다.

- Bean Validation `Configuration`은 이제 `ValidationConfigurationCustomizer` 빈을 정의함으로써 커스터마이즈 될 수 있습니다.

- Log4j2의 `Log4jBridgeHandler`는 이제 SLF4J를 통해 라우팅하는 대신 JUL 기반 로깅을 Log4j2로 라우팅하는 데 사용됩니다.

- `MeterBinder` 인터페이스를 구현하는 빈은 이제 모든 싱글톤 빈이 초기화된 후에만 미터 레지스트리에 바인딩됩니다.

- Brave와 OpenTelemetry에 대한 `SpanCustomizer` 빈들은 이제 자동으로 구성됩니다.

- Micrometer의 `JvmCompilationMetrics`는 이제 자동으로 구성됩니다.

- `DiskSpaceHealthIndicator`는 이제 로그 메시지와 건강 상세 정보에 경로를 포함합니다.

- `DataSourceBuilder`는 이제 감싸진 `DataSource`에서 파생될 수 있습니다.

- MongoDB에 대해 여러 호스트를 `spring.data.mongodb.additional-hosts` 속성을 사용하여 구성할 수 있습니다.

- Elasticsearch의 socketKeepAlive 속성은 `spring.elasticsearch.socket-keep-alive` 속성을 사용하여 구성할 수 있습니다.

- `spring-rabbit-stream`을 사용할 때, `spring.rabbitmq.listener.type`이 `stream`인지 여부에 관계없이 `RabbitStreamTemplate`과 `Environment`가 이제 자동으로 구성됩니다.

- 기존 Kafka 주제는 `spring.kafka.admin.modify-topic-configs`를 사용하여 수정할 수 있습니다.

- `WebDriverScope`와 `WebDriverTestExecutionListener`가 공개되었으므로 사용자 정의 테스트 설정에서 `WebDriver`를 쉽게 사용할 수 있습니다.

### Deprecations in Spring Boot 3.0

- `@ConstructorBinding`이 `org.springframework.boot.context.properties` 패키지에서 `org.springframework.boot.context.properties.bind`로 이동되었습니다.

- `JsonMixinModule` 스캔 기반 생성자가 deprecated 처리되었습니다.

- `ClientHttpRequestFactorySupplier`는 `ClientHttpRequestFactories`로 대체해야 합니다.

- 쿠키의 `comment` 속성은 더 이상 지원되지 않습니다.

- `RestTemplateExchangeTagsProvider`, `WebClientExchangeTagsProvider`, `WebFluxTagsProvider`, `WebMvcTagsProvider` 및 관련 클래스들은 `ObservationConvention`에 해당하는 것으로 대체되었습니다.

- `HealthContributor` `@Configuration` 기본 클래스의 인수 없는 생성자가 deprecated 처리되었습니다.

- `DefaultTestExecutionListenersPostProcessor`와 `SpringBootDependencyInjectionTestExecutionListener`는 Spring Framework의 `ApplicationContextFailureProcessor`를 대신하여 deprecated 처리되었습니다.

- `management.metrics.export.<product>` 속성은 deprecated 처리되었으며, 대체는 `management.<product>.metrics.export`입니다.

- `management.prometheus.metrics.export.pushgateway.shutdown-operation`의 `push` 설정은 `post`를 대신하여 deprecated 처리되었습니다.

- `@AutoConfigureMetrics`는 `@AutoConfigureObservability`를 대신하여 deprecated 처리되었습니다.
