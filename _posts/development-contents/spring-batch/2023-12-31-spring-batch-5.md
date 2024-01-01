---
title: Spring Batch 5
author: June
date: 2023-12-31 11:00:00 +0900
categories: [개발 게시물, Spring Boot]
tags: [프레임워크, spring, springboot, spring boot]
toc: true
math: true
mermaid: true
comments: true
---
## 들어가며

이 글은 [Spring Batch 5.0 Goes GA!](https://spring.io/blog/2022/11/24/spring-batch-5-0-goes-ga)를 번역 및 이해를 위한 일부 내용을 수정 및 덧 붙인 글입니다.

<!-- markdownlint-disable-next-line MD026 -->
## Spring Batch 5.0 Goes GA!

2022년 11월 Spring Batch가 출시되어, Maven Central에서 정식 버전으로 제공됩니다. Spring Batch 5는 50명 이상의 기여자가 참여한 수십 가지 개선 사항, 기능 및 버그 수정을 포함하여 2년간의 작업의 정점입니다!

이 블로그 게시물에서는 이 새로운 세대의 프레임워크의 주요 특징을 살펴봅니다. 모든 변경 사항에 대한 자세한 내용은 [릴리스 노트](https://github.com/spring-projects/spring-batch/releases/tag/v5.0.0)와 [마이그레이션 가이드](https://github.com/spring-projects/spring-batch/wiki/Spring-Batch-5.0-Migration-Guide)의 업그레이드 지침에서 확인할 수 있습니다.

### What's new?

- 최소 자바 버전 변경
- 주요 의존성 업그레이드
- 완전한 GraalVM 네이티브 지원
- Micrometer의 새로운 Observation API 도입
- 실행 컨텍스트 메타데이터 개선
- 새로운 기본 실행 컨텍스트 직렬화 형식
- SystemCommandTasklet 개선
- 잡 파라미터로 모든 타입을 사용 가능하도록 지원 추가
- 잡 파라미터 변환 개선
- EnableBatchProcessing에서 새로운 어노테이션 속성
- 인프라 빈을 위한 새로운 구성 클래스
- JobExplorer 및 JobOperator에서의 트랜잭션 지원
- EnableBatchProcessing으로 JobOperator 자동 등록
- 테스트 유틸리티 구성 업데이트
- JUnit Jupiter로의 마이그레이션
- 자바 레코드 지원 개선
- 기본적으로 UTF-8
- 자바 8 기능 업데이트
- 새로운 Maven Bill of Materials
- 완전한 MariaDB 지원
- 작업 저장소로서 SAP HANA 지원
- 개선된 문서화

#### New Java version baseline: 최소 자바 버전이 변경

Spring Batch는 자바 버전 및 타사 종속성에 대해 Spring Framework의 기준을 따릅니다. Spring Batch 5에서는 Spring Framework 버전이 Spring Framework 6으로 업그레이드되어, Java 17이 필요합니다. 그 결과, Spring Batch에 대한 자바 버전 요구 사항도 Java 17로 변경되었습니다.

#### Major dependencies upgrade: 주요 의존성 업그레이드

Spring Batch가 사용하는 타사 라이브러리의 지원되는 버전과의 통합을 계속하기 위해, Spring Batch 5는 다음 버전으로 전반적인 의존성을 업데이트합니다:

- Spring Framework 6
- Spring Integration 6
- Spring Data 3
- Spring AMQP 3
- Spring for Apache Kafka 3
- Micrometer 1.10

이 릴리스는 또한 다음으로의 마이그레이션을 표시합니다:

- Jakarta EE 9
- Hibernate 6

#### Full GraalVM native support: 완전한 GraalVM 네이티브 지원

GraalVM 네이티브 이미지 컴파일러를 사용하여 Spring Batch 애플리케이션을 네이티브 실행 파일로 컴파일하는 지원을 제공하기 위한 노력은 v4.2에서 시작되었으며, v4.3에서 실험적으로 출시되었습니다.

이번 릴리스에서는, GraalVM을 사용하여 Spring Batch 애플리케이션을 네이티브로 컴파일하기 위한 필요한 사전 처리와 런타임 힌트를 제공함으로써 네이티브 지원이 크게 개선되었습니다.

이 블로그 게시물에서는, 우리가 이 분야에서 작업해온 일부 벤치마크를 공유하고자 합니다. 다음 벤치마크는 [Spring Native](https://github.com/spring-projects/spring-aot-smoke-tests) 샘플 프로젝트의 [배치](https://github.com/spring-projects/spring-aot-smoke-tests/tree/main/batch/batch) 샘플을 기반으로 하고 있으며, 일반 JVM과 네이티브 실행 파일로 실행된 동일한 배치 애플리케이션의 시작 시간과 총 실행 시간의 비교를 보여줍니다:

![perf-native](/posts/development-contents/spring-batch/perf-native.png)

여기에 표시된 값은 다음 소프트웨어 및 하드웨어 설정을 사용하여 샘플을 10번 실행한 평균입니다:

- JVM: OpenJDK 버전 "17" 2021-09-14
- GraalVM: OpenJDK 런타임 환경 GraalVM CE 22.0.0.2
- MacOS BigSur v11.6.2 (CPU: 2,4 GHz 8-Core 인텔 코어 i9, 메모리: 32 GB 2667 MHz DDR4)

이 벤치마크에 따르면, 네이티브 Spring Batch 애플리케이션은 시작 시 두 배 더 빠르고, 런타임에서 거의 열 배 더 빠릅니다! 이것은 정말로 클라우드 네이티브 배치 워크로드에 있어 게임 체인저입니다!

#### Introduction of the new Observation API from Micrometer: Micrometer의 새로운 Observation API 도입

Micrometer 1.10으로의 업그레이드와 함께, 이제 배치 메트릭 외에 배치 추적도 얻을 수 있습니다. Spring Batch는 각 잡마다 스팬을 생성하고, 잡 내의 각 스텝마다 스팬을 생성합니다. 이 추적 메타데이터는 [Zipkin](https://zipkin.io/)과 같은 대시보드에서 수집되고 조회될 수 있습니다.

또한, 이 릴리스는 새로운 메트릭을 도입합니다:

- `job.launch.count`: JobLauncher를 통해 시작된 잡의 수를 보고하는 `Counter`입니다. 이는 배치 잡이 지속적으로 실행되는 JVM에서 예약되고 실행되는 환경에서 편리할 수 있습니다.
- `step.active`: LongTaskTimer 타입의 이 메트릭은 특정 잡에서 현재 활성화된(즉, 실행 중인) 스텝을 보고합니다. 이 메트릭은 잡에 여러 스텝이 있고 현재 어느 스텝에서 처리가 이루어지고 있는지 알고 싶은 상황에서 유용합니다.

#### Execution context Meta-data improvement: 실행 컨텍스트 메타데이터 개선

이미 실행 컨텍스트에 런타임 정보(스텝 타입, 재시작 플래그 등)를 지속적으로 저장하는 것 외에도, 이 릴리스는 실행 컨텍스트를 직렬화하는 데 사용된 Spring Batch 버전이라는 중요한 세부 사항을 실행 컨텍스트에 추가합니다.

이것은 사소해 보일 수 있지만, 실행 컨텍스트의 직렬화 및 역직렬화와 관련된 업그레이드 문제를 디버깅할 때 엄청난 추가 가치가 있습니다.

#### New default execution context serialization format: 새로운 기본 실행 컨텍스트 직렬화 형식

이 릴리스에서는 `DefaultExecutionContextSerializer`가 Base64로 컨텍스트를 직렬화/역직렬화하도록 업데이트되었습니다.

또한, `@EnableBatchProcessing` 또는 `DefaultBatchConfiguration`에 의해 구성된 기본 `ExecutionContextSerializer`가 `JacksonExecutionContextStringSerializer`에서 `DefaultExecutionContextSerializer`로 변경되었습니다. Jackson에 대한 의존성은 이제 선택적입니다. `JacksonExecutionContextStringSerializer`를 사용하려면, `jackson-core`를 클래스패스에 추가해야 합니다.

#### SystemCommandTasklet improvement: SystemCommandTasklet 개선

이번 릴리스에서 `SystemCommandTasklet`이 재검토되었으며 다음과 같이 변경되었습니다:

- 명령 실행을 tasklet 실행과 분리하기 위해 `CommandRunner`라는 새로운 전략 인터페이스가 도입되었습니다. 기본 구현은 `java.lang.Runtime#exec` API를 사용하여 시스템 명령을 실행하는 `JvmCommandRunner`입니다. 이 인터페이스는 다른 API를 사용하여 시스템 명령을 실행하기 위해 구현될 수 있습니다.
- 명령을 실행하는 메소드는 이제 명령과 그 인수를 나타내는 `String` 배열을 받아들입니다. 명령을 토큰화하거나 사전 처리를 할 필요가 없습니다. 이 변경은 API를 더 직관적이고 오류가 발생하기 쉽지 않게 만듭니다.

#### Support for all types of parameters as job parameters: 잡 파라미터로 모든 타입을 사용 가능하도록 지원 추가

버전 4까지 Spring Batch는 `long`, `double`, `String`, `Date`의 4가지 타입만을 잡 파라미터로 사용할 수 있도록 지원했습니다. 이는 프레임워크 측에서 잡 파라미터 처리를 단순화하는 데 편리했지만 사용자 측에서 제한적일 수 있습니다. 예를 들어, `boolean`이나 사용자 정의 타입을 잡 파라미터로 사용하고 싶다면 어떻게 해야 할까요? 이것은 Spring Batch에서 지원하는 타입 중 하나로 추가 변환을 필요로 했으며, 이는 사용자들에게 불편함을 안겨주었습니다.

이 릴리스에서는 어떤 타입이든 잡 파라미터로 사용할 수 있도록 지원을 추가했습니다. 이 개선 뒤에 있는 주요 변경은 다음과 같습니다:

```java
---public class JobParameter implements Serializable {
+++public class JobParameter<T> implements Serializable {

---   private Object parameter;
+++   private T value;

---   private ParameterType parameterType;
+++   private Class<T> type;

}
```

이 변경은 잡 파라미터가 데이터베이스에 지속되는 방식에 영향을 미칩니다. 데이터베이스 스키마 변경에 대해서는 [마이그레이션 가이드](https://github.com/spring-projects/spring-batch/wiki/Spring-Batch-5.0-Migration-Guide#column-change-in-batch_job_execution_params)를 확인하십시오. 파라미터 타입의 정규화된 이름은 값과 함께 `String`으로 유지됩니다. 문자열 리터럴은 표준 스프링 변환 서비스를 사용하여 파라미터 타입으로 변환됩니다. 표준 변환 서비스는 사용자 특정 유형을 문자열 리터럴로 변환하는 데 필요한 변환기로 강화될 수 있습니다.

#### Job parameter conversion improvement: 잡 파라미터 변환 개선

v4에서 잡 파라미터의 기본 표기법은 다음과 같습니다:

```text
[+|-]parameterName(parameterType)=parameterValue
```

여기서 `parameterType`은 `[string,long,double,date]` 중 하나입니다. 이 표기법은 간결했지만 환경 변수와 잘 어울리지 않고 Spring Boot와 친숙하지 않다는 여러 제한이 있었습니다.

v5에서는 기본 표기법을 다음과 같이 변경했습니다:

```text
parameterName=parameterValue,parameterType,identificationFlag
```

여기서 `parameterType`은 파라미터 타입의 완전한 이름입니다. 예를 들어, 다음 키/값 쌍:

```text
schedule.date=2022-12-12,java.time.LocalDate
```

은 `java.time.LocalDate` 타입의 잡 파라미터로 변환되며, 값은 `2022-12-12`입니다. 식별 플래그는 선택 사항이며 기본값은 `true`입니다. 이 새로운 기본 표기법은 대부분의 사용 사례에 적합하지만, 값이 쉼표를 포함하는 경우에는 편리하지 않을 수 있습니다. 이러한 이유로, 우리는 Spring Boot의 [Json Application Properties](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.application-json)에서 영감을 받은 새로운 "확장" 표기법을 도입했으며, 다음과 같이 지정됩니다:

```text
parameterName='{"value": "parameterValue", "type":"parameterType", "identifying": "booleanValue"}'
```

여기서 `parameterType`은 파라미터 타입의 완전한 이름입니다. Spring Batch는 이 표기법을 지원하기 위해 `JsonJobParametersConverter`를 제공합니다. 물론 `JobParametersConverter` 전략 인터페이스를 구현하고 작업 저장소 및 작업 탐색기에 사용자 정의 구현을 등록함으로써 다른 표기법을 지원할 수 있습니다.

우리는 Spring Batch에서 잡 파라미터 처리의 이러한 두 가지 주요 변경 사항이 더 편리하고 유연하며 오류가 발생하기 쉽지 않다고 믿습니다.

#### New annotation attribute in EnableBatchProcessing: EnableBatchProcessing에서 새로운 어노테이션 속성

이번 릴리스에서, `@EnableBatchProcessing` 어노테이션은 배치 인프라 빈을 구성하는 데 사용해야 할 컴포넌트와 파라미터를 지정하는 새로운 속성을 도입했습니다. 예를 들어, 이제 작업 저장소에서 Spring Batch가 구성해야 할 데이터 소스와 트랜잭션 관리자를 지정할 수 있습니다. 다음 코드 조각은 그러한 구성을 수행하는 새로운 방법을 보여줍니다:

```java
@Configuration
@EnableBatchProcessing(dataSourceRef = "batchDataSource", transactionManagerRef = "batchTransactionManager")
public class MyJobConfiguration {

    @Bean
    public Job job(JobRepository jobRepository) {
        return new JobBuilder("myJob", jobRepository)
            //define job flow as needed
            .build();
    }

}
```

이 예에서, `batchDataSource`와 `batchTransactionManager`는 애플리케이션 컨텍스트의 빈으로서 작업 저장소와 작업 탐색기를 구성하는 데 사용됩니다. 더 이상 사용자 정의 `BatchConfigurer`를 정의할 필요가 없으며, 이것은 이 릴리스에서 제거되었습니다. 예를 들어, Spring Batch v4에서 사용자 정의 실행 컨텍스트 직렬화를 제공하는 것은 다음과 같이 사용자 정의 `BatchConfigurer`를 제공함으로써 가능했습니다:

```java
@Configuration
@EnableBatchProcessing
public class MyJobConfigWithCustomSerializer {

    @Bean
    public BatchConfigurer batchConfigurer() {
        return new DefaultBatchConfigurer() {
            @Override
            public JobRepository getJobRepository() {
                JobRepositoryFactoryBean factory = new JobRepositoryFactoryBean();
                factory.setSerializer(createCustomSerializer());
                // set other properties on the factory bean
                try {
                    factory.afterPropertiesSet();
                    return factory.getObject();
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }

            @Override
            public JobExplorer getJobExplorer() {
                JobExplorerFactoryBean factoryBean = new JobExplorerFactoryBean();
                factoryBean.setSerializer(createCustomSerializer());
                // set other properties on the factory bean
                try {
                    factoryBean.afterPropertiesSet();
                    return factoryBean.getObject();
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }

            private ExecutionContextSerializer createCustomSerializer() {
                Jackson2ExecutionContextStringSerializer serializer = new Jackson2ExecutionContextStringSerializer();
                // customize serializer
                return serializer;
            }
        };
    }

}
```

Spring Batch v5에서는 사용자 정의 직렬화를 다음과 같이 제공할 수 있습니다:

```java
@Configuration
@EnableBatchProcessing(executionContextSerializerRef = "myCustomSerializer")
public class MyJobConfigWithCustomSerializer {

    @Bean
    public Job job(JobRepository jobRepository) {
        return new JobBuilder("myJob", jobRepository)
                //define job flow as needed
                .build();
    }
    
    @Bean
    public ExecutionContextSerializer myCustomSerializer() {
        Jackson2ExecutionContextStringSerializer serializer = new Jackson2ExecutionContextStringSerializer();
        // customize serializer
        return serializer;
    }

}
```

우리는 이 새로운 Spring Batch 구성 방식이 더 직관적이고 간단하며 오류가 발생하기 쉽지 않다고 믿습니다.

#### New configuration classes for infrastructure beans: 인프라 빈을 위한 새로운 구성 클래스

이번 릴리스에서는 인프라 빈을 구성하기 위해 `@EnableBatchProcessing` 대신 사용할 수 있는 새로운 구성 클래스인 `DefaultBatchConfiguration`을 사용할 수 있습니다. 이 클래스는 기본 구성으로 인프라 빈을 제공하며 필요에 따라 사용자 정의할 수 있습니다. 다음 코드 조각은 이 클래스의 전형적인 사용 예를 보여줍니다:

```java
@Configuration
class MyJobConfiguration extends DefaultBatchConfiguration {

  @Bean
  public Job job(JobRepository jobRepository) {
    return new JobBuilder("myJob", jobRepository)
        //define job flow as needed
        .build();
  }

}
```

이 예에서, `Job` 빈 정의에 주입된 `JobRepository` 빈은 `DefaultBatchConfiguration` 클래스에서 정의됩니다. 해당 getter를 오버라이딩하여 사용자 정의 파라미터를 지정할 수 있습니다. 예를 들어, 다음 예제는 작업 저장소 및 작업 탐색기에서 사용되는 기본 문자 인코딩을 재정의하는 방법을 보여줍니다:

```java
@Configuration
class MyJobConfiguration extends DefaultBatchConfiguration {

  @Bean
  public Job job(JobRepository jobRepository) {
    return new JobBuilder("job", jobRepository)
        // define job flow as needed
        .build();
  }

  @Override
  protected Charset getCharset() {
    return StandardCharsets.ISO_8859_1;
  }
}
```

#### Transaction support in JobExplorer and JobOperator: JobExplorer 및 JobOperator에서의 트랜잭션 지원

이번 릴리스는 `JobExplorerFactoryBean`을 통해 생성된 `JobExplorer`에 트랜잭션 지원을 도입합니다. 이제 배치 메타데이터를 조회할 때 사용할 트랜잭션 관리자를 지정할 수 있습니다. 또한 트랜잭션 속성을 사용자 정의할 수도 있습니다. 같은 트랜잭션 지원이 새로운 팩토리 빈인 `JobOperatorFactoryBean`을 통해 `JobOperator`에도 추가되었습니다.

#### JobOperator auto-registration with EnableBatchProcessing: EnableBatchProcessing으로 JobOperator 자동 등록

버전 4부터 `@EnableBatchProcessing`은 Spring Batch 작업을 시작하기 위해 필요한 기본 인프라 빈을 제공했습니다. 그러나 작업 실행을 중지, 재시작 및 포기하는 주요 진입점인 작업 운영자 빈을 등록하지 않았습니다.

이러한 유틸리티는 작업 시작만큼 자주 사용되지 않지만, 애플리케이션 컨텍스트에 작업 운영자를 자동으로 추가하는 것은 최종 사용자가 수동으로 빈을 구성하는 것을 피하는 데 유용할 수 있습니다.

#### Test utility configuration update: 테스트 유틸리티 구성 업데이트

버전 4.3까지 `JobLauncherTestUtils`는 테스트 설정을 용이하게 하기 위해 테스트 대상 작업을 자동 연결했습니다. 그러나 테스트 컨텍스트에서 여러 작업이 정의된 경우는 어떻게 될까요? 또는 아예 `Job` 빈이 정의되지 않은 경우는 어떨까요? 이러한 자동 연결은 대부분의 경우에 편리했지만, 위에서 언급한 상황에서 여러 문제를 일으켰습니다. 이번 릴리스에서는 커뮤니티의 피드백을 바탕으로 `JobLauncherTestUtils`에서 어떤 작업도 자동 연결하지 않기로 결정했습니다.

마찬가지로, `JobRepositoryTestUtils`는 애플리케이션 컨텍스트에서 `DataSource`를 자동 연결했습니다. 또한, 테스트 컨텍스트에 데이터 소스가 없거나 여러 데이터 소스가 정의된 경우는 어떨까요? 이 버전에서 `JobRepositoryTestUtils`는 데이터 소스와 같은 저장소의 구현 세부 사항을 다룰 필요 없이 `JobRepository` 인터페이스에 대해 작동하도록 업데이트되었습니다.

테스트 컨텍스트에서 이러한 유틸리티 빈을 수동으로 정의하거나 `@SpringBatchTest`를 통해 가져올 경우, 테스트 컨텍스트에 이러한 타입의 여러 빈이 정의된 경우 테스트 대상 작업 또는 테스트 데이터 소스를 수동으로 설정해야 합니다.

#### Migration to JUnit Jupiter: JUnit Jupiter로의 마이그레이션

이번 릴리스에서는 Spring Batch의 전체 테스트 스위트가 JUnit 5로 마이그레이션되었습니다. 이것은 최종 사용자에게 직접적인 영향을 미치지는 않지만, Batch 팀과 커뮤니티 기여자들이 JUnit의 차세대 버전을 사용하여 더 나은 테스트를 작성하는 데 도움이 됩니다.

#### Java records support improvement: 자바 레코드 지원 개선

청크 지향 단계에서 항목으로 자바 레코드를 지원하는 것은 처음에 v4.3에서 도입되었지만, v4가 자바 8을 기준으로 하기 때문에 그 지원은 제한적이었습니다. 자바 8에서는 레코드조차도 아직 미리보기 단계에 있지 않았습니다. 초기 지원은 자바 16에서 확정된 `java.lang.Record` API에 대한 접근 없이 리플렉션 트릭을 사용하여 자바 레코드를 생성하고 데이터로 채우는 것을 기반으로 했습니다.

이제 v5가 자바 17을 기준으로 삼으면서, 우리는 프레임워크의 다양한 부분에서 `java.lang.Record` API를 활용하여 Spring Batch에서 레코드 지원을 개선했습니다. 예를 들어, `FlatFileItemReaderBuilder`는 이제 항목 유형이 레코드인지 일반 클래스인지를 감지하고 해당 `FieldSetMapper` 구현을 그에 따라 구성할 수 있습니다(레코드의 경우 `RecordFieldSetMapper`, 일반 클래스의 경우 `BeanWrapperFieldSetMapper`). 여기서의 목표는 필요한 `FieldSetMapper` 유형 구성을 사용자에게 투명하게 만드는 것입니다. 같은 기능이 `FlatFileItemWriterBuilder`에도 구현되어, 항목 유형에 따라 `RecordFieldExtractor` 또는 `BeanWrapperFieldExtractor`를 구성합니다.

#### UTF-8 by default: 기본적으로 UTF-8

여러 해에 걸쳐 프레임워크의 다양한 영역에서 문자 인코딩과 관련된 여러 문제가 보고되었습니다. 예를 들어, 파일 기반 아이템 리더와 라이터 간의 일관되지 않은 기본 인코딩, 실행 컨텍스트에서 다중 바이트 문자를 처리할 때 직렬화/역직렬화 문제 등이 있습니다.

[JEP 400](https://openjdk.java.net/jeps/400)과 [UTF-8 manifesto](https://utf8everywhere.org/)의 정신에 따라, 우리는 프레임워크의 모든 영역에서 기본 인코딩을 `UTF-8`로 변경하고 적절한 곳에서 이 기본값을 구성 가능하게 만들었습니다.

#### Java 8 features update: 자바 8 기능 업데이트

이 주요 릴리스를 계기로, 자바 8+의 기능을 활용하여 코드 베이스를 개선했습니다. 예를 들어:

- 인터페이스에서 기본 메소드를 사용하고 "support" 클래스를 더 이상 사용하지 않도록 하기 ([issue 3924](https://github.com/spring-projects/spring-batch/issues/3924) 참조)
- 공개 API에서 적절한 곳에 `@FunctionalInterface` 추가 ([issue 4107](https://github.com/spring-projects/spring-batch/issues/4107) 참조)
- Date 및 Time API의 타입을 잡 파라미터로 사용하는 것을 지원 ([issue 1035](https://github.com/spring-projects/spring-batch/issues/1035) 참조)

#### New Maven Bill of Materials: 새로운 Maven Bill of Materials

이 기능은 여러 번 요청되어 왔던 기능으로, 이번 릴리스에서 마침내 제공되었습니다. 이제 새로 추가된 Maven BOM을 사용하여 일관된 버전 번호로 Spring Batch 모듈을 가져올 수 있습니다.

#### Full MariaDB support: 완전한 MariaDB 지원

v4.3까지, Spring Batch는 MariaDB를 MySQL로 간주하여 지원했습니다. 이번 릴리스에서 MariaDB는 별도의 데이터베이스 제품으로 취급되며, 자체 DDL 스크립트와 `DataFieldMaxValueIncrementer`를 갖습니다.

#### SAP HANA support as a job repository: 작업 저장소로서 SAP HANA 지원

SAP Hana는 이제 Spring Batch에서 공식적으로 작업 저장소로 지원됩니다.

#### Improved documentation: 개선된 문서화

이번 릴리스에서는 [Spring Asciidoctor Backend](https://github.com/spring-io/spring-asciidoctor-backends)를 사용하여 문서가 업데이트되었습니다. 이 백엔드는 포트폴리오의 모든 프로젝트가 동일한 문서 스타일을 따르도록 보장합니다. 다른 프로젝트와 일관성을 유지하기 위해, 이번 릴리스에서 Spring Batch의 참조 문서는 이 백엔드를 사용하여 업데이트되었습니다. 새로운 버전의 참조 문서는 [여기](https://docs.spring.io/spring-batch/docs/5.0.0/reference/html/)에서 확인할 수 있습니다.

### What's deprecated or removed?: 무엇이 폐지되었거나 제거되었나요?

이 주요 릴리스에서는 이전 버전에서 폐지된 모든 API가 제거되었습니다. 또한, 일부 API는 v5.0에서 폐지되었으며 v5.2에서 제거될 예정입니다. 마지막으로, 실용적인 이유로 일부 API는 폐지 없이 이동되거나 제거되었습니다. 모든 폐지된 API의 목록은 [마이그레이션 가이드](https://github.com/spring-projects/spring-batch/wiki/Spring-Batch-5.0-Migration-Guide#deprecated-apis)를 참조하십시오.

#### SQLFire Support Removal: SQLFire 지원 제거

SQLFire는 2014년 11월 1일부로 EOL이 발표되었습니다. SQLFire를 작업 저장소로서의 지원은 버전 v4.3에서 폐지되었고 버전 v5.0에서 제거되었습니다.

#### GemFire support removal: GemFire 지원 제거

Spring Data for Apache Geode의 [지원 중단 결정](https://github.com/spring-projects/spring-data-geode#notice)에 따라, Spring Batch에서 Apache Geode에 대한 지원이 제거되었습니다.

코드는 커뮤니티 주도 노력으로 [spring-batch-extensions](https://github.com/spring-projects/spring-batch-extensions) 저장소로 이동되었습니다.

#### JSR-352 Implementation Removal: JSR-352 구현 제거

채택률이 저조하여, 이번 릴리스에서 JSR-352의 구현이 중단되었습니다.

### What's been fixed?: 무엇이 수정되었나요?

일부 버그는 파괴적인 변경 없이는 수정할 수 없습니다. 우리는 이 주요 릴리스의 기회를 통해 이러한 버그들을 수정합니다. 이번 릴리스에서 수정된 40개 이상의 버그 목록은 [릴리스 노트](https://github.com/spring-projects/spring-batch/releases/tag/5.0.0)를 참조해 주세요!

### Feedback and contributions: 피드백 및 기여

이 대규모 릴리스에 참여해주신 한 모든 기여자분들께 감사의 말씀을 드립니다! 이번 릴리스는 일반적으로 놀라운 스프링 커뮤니티와 특히 스프링 배치 커뮤니티의 도움 없이는 불가능했을 것입니다. 이 주요 릴리스에 대한 여러분의 피드백과 배치 인프라를 어떻게 개선할 수 있는지 듣고 싶습니다. 귀하의 피드백은 [Github](https://github.com/spring-projects/spring-batch/issues), [Twitter](https://twitter.com/springbatch), [StackOverflow](https://stackoverflow.com/questions/tagged/spring-batch)에 제출해 주세요.

### What's next?: 다음은 무엇인가요?

스프링 배치의 차세대 버전을 방금 출시하였지만, 우리는 여전히 다음 버전에 대해 작업 중이거나 계획 중인 수많은 아이디어와 기능을 가지고 있습니다. 예를 들면:

- 청크 지향 처리 모델의 새로운 구현
- 자바 19의 가상 스레드를 기반으로 한 새로운 동시성 모델
- MongoDB를 기반으로 한 새로운 작업 저장소 구현
- 그리고 더 많은 것들!

우리는 가까운 미래에 우리의 완전한 로드맵을 공유하고 이 새로운 기능들의 초기 개발 및 테스트 단계에 참여하는 방법을 보여줄 것입니다. 계속 지켜봐 주세요!
