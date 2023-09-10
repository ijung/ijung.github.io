---
title: 스프링 부트 CRUD API 만들기(kotlin)
author: June
date: 2023-09-09 18:30:00 +0900
categories: [강좌]
tags: [강좌]
toc: true
math: true
mermaid: true
comments: true
---

1. MySql 설치 및 실행
    - [도커를 이용해 mysql 설치 및 실행하기](/posts/development-course/install-mysql-using-docker/)
1. 스프링 부트 프로젝트 만들기
    - [스프링 부트 프로젝트 만들기](/posts/development-course/create-spring-boot-project/)
1. 스프링 부트 프로젝트에 MySql 연동하기
    - gradle.build.kts에 의존성 추가
        - `spring-boot-starter-data-jpa`와 `mysql-connector-j` 의존성을 추가한다.
        - MySql 5.x 이하 버전 등 낮은 버전을 사용할 경우 `mysql-connector-java`를 사용한다.

        ```kotlin
        // https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-jpa
        implementation("org.springframework.boot:spring-boot-starter-data-jpa")

        // https://mvnrepository.com/artifact/com.mysql/mysql-connector-j
        implementation("com.mysql:mysql-connector-j")
        ```

    - MySql 연결 정보 추가
        - `spring.datasource.url`에 MySql 연결 정보를 추가한다.
        - `spring.datasource.username`에 MySql 사용자 이름을 추가한다.
        - `spring.datasource.password`에 MySql 사용자 비밀번호를 추가한다.

    - 설정(`application.yml` or `application.properties`) 추가
        - `spring.jpa.properties.hibernate.dialect` 설정을 합니다.
            - `spring.jpa.properties.hibernate.dialect` 속성은 Hibernate가 사용할 SQL 방언(dialect)을 지정하는 데 사용됩니다. SQL 방언은 특정 데이터베이스 벤더에 대한 SQL 쿼리 구문의 차이를 다룹니다.
            - MySql 버전에 따라 `spring.jpa.properties.hibernate.dialect` 설정을 추가한다.
            - MySql 5.x 이하 버전 등 낮은 버전을 사용할 경우 `org.hibernate.dialect.MySQL5InnoDBDialect`를 사용한다.
            - MySql 8.x 이상 버전 등 높은 버전을 사용할 경우 `org.hibernate.dialect.MySQL8Dialect`를 사용한다.
        - 기타
            - `spring.jpa.show-sql`
                - true 설정 시, JPA 쿼리문 확인이 가능하다.
            - `spring.jpa.hibernate.ddl-auto`
                - Hibernate ORM 프레임워크를 사용하여 데이터베이스 스키마를 자동으로 생성, 업데이트 또는 검증하는 방법을 지정하는 데 사용된다.
                - none: Hibernate는 어떤 종류의 데이터베이스 스키마 생성도 실행하지 않습니다.
                - validate: Hibernate는 클래스와 테이블이 올바르게 매핑되었는지 확인하기 위해 현재 데이터베이스 스키마를 검증만 수행합니다. 불일치가 발견되면 예외가 발생합니다.
                - update: Hibernate는 데이터베이스 스키마를 현재 매핑에 맞게 업데이트합니다. 즉, 새로운 엔터티나 프로퍼티가 추가되면 해당 테이블과 컬럼을 생성합니다. 이렇게 하면 데이터베이스 스키마가 엔터티 클래스에 정의된 형식과 일치하도록 유지할 수 있습니다.
                - create: Hibernate는 기존 데이터베이스 스키마를 삭제하고, 엔터티 매핑을 기반으로 새 스키마를 생성합니다. 이렇게 하면 데이터베이스에 저장된 모든 데이터가 삭제됩니다.
                - create-drop: "create" 옵션과 유사하지만, SessionFactory가 닫힐 때 데이터베이스 스키마를 삭제합니다. 이는 주로 통합 테스트에서 사용됩니다.
            - `spring.jpa.properties.hibernate.format_sql`
                - Hibernate가 생성하는 SQL 쿼리의 로깅 형식을 제어합니다. 이 설정을 true로 설정하면 Hibernate가 생성하는 SQL 쿼리를 더 읽기 쉽게 형식화하여 로그에 출력합니다.
            - `logging.level.org.hibernate.type.descriptor.sql`
                - Hibernate가 SQL 쿼리를 로깅할 때 사용할 로깅 수준을 제어합니다.
                - 이 설정을 TRACE로 설정하면 Hibernate가 SQL 쿼리를 로그에 출력합니다.
                - 이 때, 쿼리의 파라미터 값도 함께 출력됩니다.
        - example

            ```yml
            # application.yml
            spring:
                datasource:
                    url: jdbc:mysql://localhost:3306/demo_db
                    username: root
                    password: admin
                jpa:
                    properties:
                    hibernate:
                        dialect: org.hibernate.dialect.MySQL8Dialect
                        format_sql: true
                    hibernate:
                        ddl-auto: update
                    show_sql: true
            ```

    - entity 설정
        - `@Entity` 어노테이션을 추가한다.
        - 테이블, 컬럼 정보 등을 추가하여 entity를 설정한다.
        - 레코드 생성 시간, 수정 시간을 추가한다.
            - 생성 시간은 `@CreatedDate` 어노테이션을 추가한다.
            - 수정 시간은 `@LastModifiedDate` 어노테이션을 추가한다.
            - `@EntityListeners(AuditingEntityListener::class)` 어노테이션을 추가한다.
            - `@EnableJpaAuditing` 어노테이션을 Application Class에 추가한다.
        - example

            ```kotlin
            @Entity
            @Table(name = "messages")
            @EntityListeners(AuditingEntityListener::class)
            data class MessageEntity(
                @Id
                @GeneratedValue(strategy = GenerationType.IDENTITY)
                @get:JsonProperty
                var id: Long? = null,

                @Column(name = "user_id", nullable = false)
                var userId: Long,

                @Column(name = "message", nullable = false)
                var message: String,

                @CreatedDate
                @Column(name = "created_date", nullable = true, updatable = false)
                @get:JsonProperty
                var createdDate: LocalDateTime? = null,

                @LastModifiedDate
                @Column(name = "last_modified_date", nullable = true)
                @get:JsonProperty
                var lastModifiedDate: LocalDateTime? = null
            )
            ```

            ```kotlin
            @SpringBootApplication
            @EnableJpaAuditing
            class DemoApplication

            fun main(args: Array<String>) {
                runApplication<DemoApplication>(*args)
            }
            ```

    - repository 설정
        - `@Repository` 어노테이션을 추가한다.
        - `CrudRepository`를 상속받는 interface를 생성한다.
            - `CrudRepository<Entity Type, Id Type>`을 상속받는다.
            - `CrudRepository`는 `save`, `findById`, `findAll`, `deleteById` 등의 메소드를 제공한다.
        - example

            ```kotlin
            @Repository
            interface MessageRepository: CrudRepository<MessageEntity, Long>
            ```

    - controller 설정
        - `@RestController` 어노테이션을 추가한다.
        - `@RequestMapping` 어노테이션을 추가한다.
            - `@RequestMapping` 어노테이션은 클래스 레벨과 메소드 레벨에 모두 사용할 수 있다.
            - 클래스 레벨에 사용할 경우, 해당 클래스의 모든 메소드에 공통적으로 적용된다.
            - 메소드 레벨에 사용할 경우, 해당 메소드에만 적용된다.
            - `@RequestMapping` 어노테이션은 `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` 등으로 대체할 수 있다.
        - `repository`를 주입받는다.
        - CRUD API를 생성한다.
        - example

            ```kotlin
            @RestController
            @RequestMapping("/messages")
            class MessageController(
                private val repository: MessageRepository
            ) {
                @PostMapping()
                fun save(@RequestBody entity: MessageEntity): MessageEntity = repository.save(entity)

                @GetMapping
                fun findAll(): List<MessageEntity> = repository.findAll().toList()

                @GetMapping("/{id}")
                fun findById(@PathVariable id: Long): MessageEntity? = repository.findByIdOrNull(id)

                @PutMapping("/{id}")
                fun save(@PathVariable id: Long, @RequestBody entity: MessageEntity): MessageEntity {
                    val originEntity = repository.findById(id).get()
                    originEntity.userId = entity.userId
                    originEntity.message = entity.message
                    return repository.save(originEntity)
                }

                @DeleteMapping("/{id}")
                @ResponseStatus(HttpStatus.NO_CONTENT)
                fun deleteById(@PathVariable id: Long) = repository.deleteById(id)

            }
            ```

[Source Code(simple 버전)](https://github.com/ijung/spring-boot-demo/tree/feature/create-crud-api/create-simple-version/base)

[Source Code(최종 버전)](https://github.com/ijung/spring-boot-demo/tree/feature/create-crud-api/base)