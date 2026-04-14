# 🏗️ Generic Spring Boot Reactive Microservice — Blueprint Prompt

> **Purpose:** Use this prompt as a complete specification to generate a production-ready Spring Boot reactive microservice with the same architectural patterns, configurations, database integration, external service connections, and cross-cutting concerns used in this project. Replace all `{PLACEHOLDER}` values with your actual service details.

---

## 📋 How to Use This Prompt

Copy the entire **"AI Generation Prompt"** section below, fill in every `{PLACEHOLDER}`, and submit it to your AI coding assistant (e.g., GitHub Copilot, ChatGPT, or similar). The resulting project will have the same structural and architectural quality as this reference service.

---

## 🤖 AI Generation Prompt

---

### CONTEXT & OBJECTIVE

Generate a **production-ready Spring Boot 3.x reactive microservice** called **`{SERVICE_NAME}`** (e.g., `my-domain-service`).

- **Group ID:** `{GROUP_ID}` (e.g., `com.mycompany.myteam`)
- **Artifact ID:** `{ARTIFACT_ID}` (e.g., `my-domain-service`)
- **Base Package:** `{BASE_PACKAGE}` (e.g., `com.mycompany.myteam.myservice`)
- **Java Version:** 17
- **Spring Boot Version:** 3.4.x
- **Build Tool:** Maven
- **Application Type:** Reactive (WebFlux), **NOT** traditional blocking MVC

The service handles **`{DOMAIN_DESCRIPTION}`** (e.g., "order management", "user profile operations", "inventory tracking"). Do **not** implement the exact same business logic as the reference service — implement business logic relevant to `{DOMAIN_DESCRIPTION}`. However, **all infrastructure, configuration, cross-cutting concerns, and architectural patterns must be identical** to the specification below.

---

### 1. PROJECT STRUCTURE

Generate the following Maven standard directory layout:

```
{ARTIFACT_ID}/
├── Dockerfile
├── pom.xml
├── README.md
├── HELP.md
├── mvnw
├── mvnw.cmd
├── Actionsfile/
│   ├── dev
│   ├── qa1
│   ├── qa2
│   ├── perf1
│   ├── stage
│   └── prod
├── docs/
│   └── {SERVICE_NAME_UPPER}_DOCUMENTATION.md
└── src/
    ├── main/
    │   ├── java/
    │   │   └── {BASE_PACKAGE_PATH}/
    │   │       ├── {MainClassName}.java               ← Spring Boot entry point
    │   │       ├── aspect/
    │   │       │   ├── Auditable.java                 ← Custom annotation
    │   │       │   ├── AuditAspect.java               ← @AfterReturning audit aspect
    │   │       │   └── LoggingAspect.java             ← @Around logging aspect
    │   │       ├── config/
    │   │       │   ├── {ServiceName}Config.java       ← Main config + security bypass
    │   │       │   ├── MetricsConfig.java             ← Micrometer TimedAspect
    │   │       │   ├── ReactiveWebServerConfig.java   ← WebClient customizer
    │   │       │   ├── SwaggerConfig.java             ← OpenAPI bean
    │   │       │   └── SwaggerProperties.java         ← Swagger config properties
    │   │       ├── constants/
    │   │       │   └── {ServiceName}Literals.java     ← All string constants
    │   │       ├── context/
    │   │       │   ├── UserContext.java               ← userId + correlationId holder
    │   │       │   ├── UserContextHolder.java         ← Reactor Context accessor
    │   │       │   └── UserContextWebFilter.java      ← WebFilter to populate context
    │   │       ├── controller/
    │   │       │   ├── BaseController.java            ← Shared response helpers
    │   │       │   └── {Domain}Controller.java        ← REST endpoints
    │   │       ├── dto/
    │   │       │   ├── {Domain}RequestDto.java
    │   │       │   ├── {Domain}ResponseDto.java
    │   │       │   ├── {ServiceName}ResponseDto.java  ← Generic wrapper response
    │   │       │   ├── ErrorDto.java
    │   │       │   └── ErrorResponse.java
    │   │       ├── entity/
    │   │       │   └── {Domain}Entity.java            ← R2DBC @Table entity
    │   │       ├── exception/
    │   │       │   └── GlobalExceptionHandler.java    ← @RestControllerAdvice
    │   │       ├── mapper/
    │   │       │   ├── {ServiceName}MapperUtil.java   ← Static mapping helpers
    │   │       │   └── I{Domain}Mapper.java           ← MapStruct interface
    │   │       ├── notification/
    │   │       │   └── mapper/
    │   │       │       ├── CoreSchemaNotificationRequestMapper.java
    │   │       │       └── KafkaNotificationEventRequestMapper.java
    │   │       ├── props/
    │   │       │   └── {ServiceName}Properties.java   ← @ConfigurationProperties
    │   │       ├── repository/
    │   │       │   └── {Domain}Repository.java        ← R2DBC reactive repository
    │   │       ├── service/
    │   │       │   ├── I{Domain}Service.java          ← Service interface
    │   │       │   ├── {Domain}ServiceImpl.java       ← Service implementation
    │   │       │   ├── IActivityAuditService.java
    │   │       │   └── ActivityAuditService.java      ← Audit trail writer
    │   │       └── util/
    │   │           ├── {Domain}Validations.java       ← Input validations (reactive)
    │   │           └── CommonUtils.java               ← Shared utility methods
    │   └── resources/
    │       ├── bootstrap.yml
    │       ├── config/
    │       │   └── application.yml
    │       ├── db/
    │       │   └── changelog/
    │       │       ├── {app}-changelog.xml             ← Liquibase master changelog
    │       │       └── changes/
    │       │           └── 1.initial_database_creation.sql
    │       └── log/
    │           ├── platform-core-logback.xml
    │           ├── platform-core-logback-local.xml
    │           └── platform-core-logback-prod.xml
    └── test/
        ├── java/
        │   └── {BASE_PACKAGE_PATH}/
        │       ├── controller/
        │       │   ├── BaseControllerTest.java
        │       │   └── {Domain}ControllerTest.java
        │       ├── service/
        │       │   └── {Domain}ServiceTest.java
        │       ├── notification/
        │       │   └── mapper/
        │       │       └── KafkaNotificationEventRequestMapperTest.java
        │       └── util/
        │           ├── {Domain}ValidationsTest.java
        │           └── CommonUtilsTest.java
        └── resources/
            └── config/
                └── application.yml                    ← Test overrides
```

---

### 2. pom.xml — DEPENDENCIES & BUILD

#### 2.1 Parent
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.4.5</version>
</parent>
```

#### 2.2 Properties (version pins)
```xml
<properties>
    <java.version>17</java.version>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <org.mapstruct.version>1.5.3.Final</org.mapstruct.version>
    <springdoc.openapi.version>2.7.0</springdoc.openapi.version>
    <spring.cloud.version>2024.0.0</spring.cloud.version>
    <swagger.annotations.version>2.2.8</swagger.annotations.version>
    <liquibase.core.version>4.22.0</liquibase.core.version>
    <jacoco.maven.plugin.version>0.8.8</jacoco.maven.plugin.version>
    <mockito-core.version>5.2.0</mockito-core.version>
    <reactor-netty-http.version>1.1.26</reactor-netty-http.version>
    <netty.handler.version>4.1.118.Final</netty.handler.version>
    <r2dbc.version>1.0.0.RELEASE</r2dbc.version>
    <maven-surefire-plugin.version>2.22.2</maven-surefire-plugin.version>
    <maven-compiler-plugin.version>3.8.1</maven-compiler-plugin.version>
    <spring-boot-maven-plugin.version>3.3.7</spring-boot-maven-plugin.version>
    <apache.tomcat.version>10.1.44</apache.tomcat.version>
    <!-- Sonar exclusions — keep consistent with JaCoCo -->
    <sonar.coverage.exclusions>
        src/test/**,**/entity/**,**/config/**,**/constants/**,**/exception/**,
        **/props/**,**/repository/**,{MainClassName}.java,**/aspect/**,**/dto/**,**/mapper/**
    </sonar.coverage.exclusions>
</properties>
```

#### 2.3 Required Dependencies

Include **all** of the following dependencies. Do not omit any:

| Dependency | Notes |
|---|---|
| `spring-boot-starter-webflux` | Reactive web stack |
| `spring-boot-starter-actuator` | Health, metrics, prometheus endpoints |
| `spring-boot-starter-aop` | Aspect-Oriented Programming support |
| `spring-boot-starter-data-r2dbc` | Reactive database access |
| `spring-boot-starter-oauth2-client` | OAuth2 client credentials flow |
| `spring-boot-starter-test` (test) | Test framework |
| `reactor-test` (test) | `StepVerifier` for reactive tests |
| `io.r2dbc:r2dbc-spi` | R2DBC SPI |
| `io.r2dbc:r2dbc-mssql` | SQL Server R2DBC driver (exclude conflicting reactor/netty) |
| `com.microsoft.sqlserver:mssql-jdbc:12.8.1.jre11` | JDBC for Liquibase migrations |
| `org.liquibase:liquibase-core:${liquibase.core.version}` | DB schema migrations |
| `org.projectlombok:lombok` (optional) | Boilerplate reduction |
| `org.mapstruct:mapstruct:${org.mapstruct.version}` | Compile-time bean mapping |
| `org.mapstruct:mapstruct-processor:${org.mapstruct.version}` (optional, compile scope) | MapStruct annotation processor |
| `javax.annotation:javax.annotation-api:1.3.2` | JSR-250 annotation support |
| `org.springdoc:springdoc-openapi-starter-webflux-ui:${springdoc.openapi.version}` | Swagger UI for WebFlux |
| `io.swagger.core.v3:swagger-annotations:${swagger.annotations.version}` | Swagger annotations |
| `org.json:json:20240303` | JSON utilities |
| `com.googlecode.json-simple:json-simple:1.1.1` | Lightweight JSON |
| `org.junit.jupiter:junit-jupiter` (test) | JUnit 5 |
| `org.junit.vintage:junit-vintage-engine` (test) | JUnit 4 bridge |
| `spring-cloud-starter-loadbalancer` | Client-side load balancing (exclude ribbon, sleuth) |
| `org.apache.tomcat.embed:tomcat-embed-core:${apache.tomcat.version}` | Tomcat override |
| `org.apache.tomcat.embed:tomcat-embed-websocket:${apache.tomcat.version}` | Tomcat websocket override |

#### 2.4 Build Plugins

Configure the following plugins:

1. **`spring-boot-maven-plugin`**
   - Set `<mainClass>{BASE_PACKAGE}.{MainClassName}</mainClass>`
   - Exclude `lombok`
   - Goals: `build-info`, `repackage`

2. **`maven-compiler-plugin`** (version `3.8.1`)
   - Source/Target: 17
   - Annotation processor paths: `mapstruct-processor`, `lombok`, `lombok-mapstruct-binding:0.2.0`
   - Compiler args: `-Amapstruct.suppressGeneratorTimestamp=true`, `-Amapstruct.defaultComponentModel=spring`

3. **`maven-surefire-plugin`** (version `2.22.2`)
   - argLine: `@{argLine} -Xms2G -Xmx4G -XX:ReservedCodeCacheSize=2048m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8`
   - `<forkCount>2</forkCount>`, `<reuseForks>true</reuseForks>`, `<runOrder>alphabetical</runOrder>`

4. **`jacoco-maven-plugin`** (version `0.8.8`)
   - Exclude: `entity`, `config`, `constants`, `props`, `exception`, `dto`, `mapper`, `aspect`, `repository`, main application class
   - Minimum instruction coverage: **80%** (`COVEREDRATIO >= 0.8`)
   - Executions: `prepare-agent`, `prepare-agent-integration`, `report` (at `prepare-package`), `check`

---

### 3. MAIN APPLICATION CLASS

```java
@SpringBootApplication
@EnableTransactionManagement
@EnableScheduling
@EnableAspectJAutoProxy
@ComponentScan(basePackages = "{TOP_LEVEL_SCAN_PACKAGE}") // e.g., "com.mycompany"
public class {MainClassName} {
    public static void main(String[] args) {
        new SpringApplicationBuilder({MainClassName}.class)
            .web(WebApplicationType.REACTIVE)
            .build()
            .run(args);
    }
}
```

> ⚠️ Always use `WebApplicationType.REACTIVE` — never `SERVLET`.

---

### 4. CONFIGURATION FILES

#### 4.1 `src/main/resources/bootstrap.yml`

```yaml
spring:
  config:
    use-legacy-processing: true
  profiles:
    active: native, local
  application:
    name: {SERVICE_NAME}
  cloud:
    server:
      bootstrap: false
      health:
        enabled: false
    native:
      searchLocations: classpath:/config
```

#### 4.2 `src/main/resources/config/application.yml`

```yaml
server:
  port: 8080
  servlet:
    context-path: /
  tomcat:
    max-connections: 10000
    accept-count: 100
    min-spare-threads: 10
    max-threads: 400
    uri-encoding: UTF-8
    accesslog:
      directory: logs/access

springdoc:
  swagger-ui:
    enabled: true
  swagger-ui.path: /swagger-ui.html

management:
  endpoint:
    prometheus:
      enabled: true
  server:
    base-path: /
  endpoints:
    web:
      exposure:
        include: info, health, metrics, loggers, mappings, configprops, env, hystrix.stream, heapdump, threaddump, refresh, prometheus
  metrics:
    enable:
      all: true
  jmx:
    metrics:
      export:
        enabled: true
  prometheus:
    metrics:
      export:
        enabled: true
        step: 60

spring:
  profiles:
    active: ${spring-profiles-active:local}
  application:
    name: {SERVICE_NAME}
  sql:
    init:
      mode: never
  r2dbc:
    url: r2dbc:sqlserver://${sql-database-hostname}:${sql-database-port}/${sql-database-name}?sendStringParametersAsUnicode=false
    username: ${sql-database-user}
    password: ${sql-database-password}
    pool:
      enabled: true
      initial-size: ${initial-size:10}
      max-size: ${max-size:30}
      max-idle-time: ${max-idle-time:50s}
      max-life-time: ${max-life-time:30m}
      max-acquire-time: ${max-acquire-time:30s}
      validation-query: SELECT 1
      metrics-enabled: true
    properties:
      preferCursoredExecution: false
  liquibase:
    change-log: classpath:db/changelog/{service-name}-changelog.xml
    enabled: true
    url: ${sql-database-url}
    user: ${sql-database-user}
    password: ${sql-database-password}
  codec:
    max-in-memory-size: 100MB
  main:
    allow-bean-definition-overriding: true
  cloud:
    loadbalancer:
      ribbon:
        enabled: false
    discovery:
      client:
        simple:
          instances:
            {downstream-service-name}:
              - serviceId: {downstream-service-name}
                uri: {downstream-service-base-url}
  security:
    oauth2:
      client:
        registration:
          {service-name}:
            authorization-grant-type: client_credentials
            client-authentication-method: client_secret_post
            provider: {service-name}
            client-id: ${service-azure-ad-client-id}
            client-secret: ${service-azure-ad-client-secret}
        provider:
          {service-name}:
            token-uri: https://login.microsoftonline.com/{tenant-id}/oauth2/token

resilience4j:
  circuitbreaker:
    configs:
      default:
        registerHealthIndicator: false

com:
  {org-name}:
    {service-short-name}:
      swagger:
        title: {Service Display Name}
        description: {Service Display Description}
      kafka:
        email-notification-producer-config:
          topic-name: {KAFKA_TOPIC_NAME}
          max-retries: ${notification-producer-max-retries:3}
          retry-delay: ${notification-producer-retry-delay:3}
      notification-events-content:
        {event-key-1}:
          event-type: {EventType1NotificationEvent}
          email-enabled: ${event1-notification-email-enabled:true}
          message-title: ${event1-notification-message-title:Default Title}
          message-body: ${event1-notification-message-body}
        {event-key-2}:
          event-type: {EventType2NotificationEvent}
          email-enabled: ${event2-notification-email-enabled:true}
          message-title: ${event2-notification-message-title:Default Title}
          message-body: ${event2-notification-message-body}
    {downstream-client-namespace}:
      client:
        {downstream-service-name}:
          api-key: ${downstream-service-api-key}
          base-url: {downstream-service-base-url}
          retry-max-attempts: 1
          retry-min-backoff: 1
          httpNonRetryableErrorCodes:
            - 404
      httpClientConnectionProviderConfig:
        poolName: http-conn-pool
        maxConnections: 200
        maxIdleTime: 1000
        maxLifeTime: 60000
        maxInMemorySize: 1048576
        metricsEnabled: true
        pendingAcquireTimeout: 60000
        pendingAcquireMaxCount: 100
        evictInBackground: 30000
        debugAllowedEnvironments:
          - 'dev'
          - 'qa1'
          - 'qa2'
```

> **Key variables to replace:** `{SERVICE_NAME}`, `{sql-database-hostname}`, `{sql-database-port}`, `{sql-database-name}`, `{downstream-service-name}`, `{downstream-service-base-url}`, `{tenant-id}`, `{KAFKA_TOPIC_NAME}`, `{org-name}`, `{service-short-name}`, `{event-key-1}`, `{event-key-2}`.

---

### 5. JAVA SOURCE CODE — CROSS-CUTTING CONCERNS

#### 5.1 Main Config Class (`{ServiceName}Config.java`)

```java
@Configuration
@Slf4j
@Import({MetricsConfig.class, ReactiveWebServerConfig.class})
@EnableConfigurationProperties({ServiceName}Properties.class)
public class {ServiceName}Config {

    private final BuildProperties buildProperties;

    @Autowired
    public {ServiceName}Config(BuildProperties buildProperties) {
        this.buildProperties = buildProperties;
    }

    // Log artifact name, version, and build time on startup
    @PostConstruct
    public void deployedBuildInfo() {
        log.info("*****Deployed Artifact Name:{},Version:{},Time:{} *****",
            buildProperties.getArtifact(), buildProperties.getVersion(), buildProperties.getTime());
    }

    // Disable Spring Security reactive filter chain
    @Bean
    public SecurityWebFilterChain securityWebFilterChain() {
        return new SecurityWebFilterChain() {
            @Override public Mono<Boolean> matches(ServerWebExchange exchange) { return Mono.just(Boolean.FALSE); }
            @Override public Flux<WebFilter> getWebFilters() { return Flux.empty(); }
        };
    }
}
```

#### 5.2 MetricsConfig.java

```java
@Configuration
@EnableAspectJAutoProxy
public class MetricsConfig {
    @Bean
    public TimedAspect timedAspect(MeterRegistry registry) {
        return new TimedAspect(registry);
    }
}
```

#### 5.3 ReactiveWebServerConfig.java

```java
@Configuration
@ConditionalOnClass(Tomcat.class)
public class ReactiveWebServerConfig {
    @Bean
    public WebClientCustomizer defaultWebClientCustomizer() {
        return builder -> builder.filter((req, exchange) -> exchange.exchange(req));
    }
}
```

#### 5.4 SwaggerConfig.java

```java
@Configuration
public class SwaggerConfig {
    @Bean
    public OpenAPI openAPI() {
        return new OpenAPI().info(new Info()
            .title("{Service Display Name} API")
            .description("{Service Display Description}")
            .version("v1"));
    }
}
```

#### 5.5 LoggingAspect.java

- Use `@Aspect` + `@Component`
- Define two pointcuts:
  - `controllerMethods()` → `execution(* {BASE_PACKAGE}.controller.*.*(..))` excluding `BaseController`
  - `serviceMethods()` → `execution(* {BASE_PACKAGE}.service.*.*(..))`
- `@Around("controllerMethods() || serviceMethods()")` → log entry with class/method name, log args at DEBUG, log exit with execution time (ms)
- Handle both `Mono<?>` and `Flux<?>` return types when logging execution time using `instanceof` checks — subscribe to reactive types to capture timing in `doOnSuccess`/`doOnError`

#### 5.6 AuditAspect.java

```java
@Aspect
@Component
@RequiredArgsConstructor
public class AuditAspect {
    private final IActivityAuditService activityAuditService;

    @AfterReturning("@annotation(auditable)")
    public void auditMethod(JoinPoint joinPoint, Auditable auditable) {
        String username = "system"; // Replace with UserContextHolder extraction
        String entityId = "unknown";
        activityAuditService.log(entityId, auditable.entity(), auditable.action(), username);
    }
}
```

#### 5.7 `@Auditable` Custom Annotation

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Auditable {
    String entity();
    String action();
}
```

#### 5.8 UserContext & WebFilter

```java
// UserContext.java
@Data @AllArgsConstructor @NoArgsConstructor
public class UserContext {
    private String userId;
    private String correlationId;
}

// UserContextHolder.java — Reactor Context-based static accessor
// UserContextWebFilter.java — WebFilter that reads X-User-Id and x-correlation-id headers
//   and writes a UserContext into the Reactor subscriber context
```

#### 5.9 GlobalExceptionHandler.java

Implement a `@RestControllerAdvice` that handles:

| Exception | HTTP Status | Notes |
|---|---|---|
| `Exception` | 500 | Generic fallback |
| `ResponseStatusException` | From exception | Use `ex.getStatusCode()` |
| `WebExchangeBindException` | 400 | Return field-level validation errors map |
| `ServerWebInputException` | 400 | Invalid input |
| `DataIntegrityViolationException` | 409 | DB constraint violation |

All handlers return `Mono<ResponseEntity<ErrorResponse>>`. Include `timestamp`, `status`, `error`, and `message` fields in `ErrorResponse`.

#### 5.10 Constants Class (`{ServiceName}Literals.java`)

A `public final class` with private constructor. Include:
- `DATE_FORMAT_PST = "America/Los_Angeles"`
- `APPLICATION_VND_{ORG}_{VERSION}_JSON = "application/vnd.{org}.v2+json"` — custom media type
- `HEADER_USER_ID = "X-User-Id"`
- `HEADER_CORRELATION_ID = "x-correlation-id"`
- Kafka serializer class constant: `STRING_SERIALIZER_CLASS`
- All domain-specific status strings, notification event keys, and placeholder tokens

---

### 6. SERVICE LAYER PATTERN

#### 6.1 Interface + Implementation

Every domain service must have:
- An interface: `I{Domain}Service.java`
- An implementation: `{Domain}ServiceImpl.java` annotated with `@Service`, `@RequiredArgsConstructor`, `@Slf4j`

All service methods must return `Mono<T>` or `Flux<T>` — **never** blocking types.

#### 6.2 Repository Service (`{Domain}RepositoryService.java`)

Create a dedicated service class that wraps all repository calls. Each method must:
- Return `Mono<T>` or `Flux<T>`
- Include `.doOnSuccess(result -> log.info(...))` and `.doOnError(error -> log.error(...))` for observability

#### 6.3 Activity Audit Service

```java
public interface IActivityAuditService {
    void log(String entityId, String entityName, String action, String userId);
}

@Service
@RequiredArgsConstructor
public class ActivityAuditService implements IActivityAuditService {
    private final ActivityAuditRepository activityAuditRepository;

    public void log(String entityId, String entityName, String action, String userId) {
        // Build ActivityAudit entity, set fields including updatedAt with PST timezone
        // Call activityAuditRepository.save(audit)
    }
}
```

---

### 7. CONTROLLER LAYER PATTERN

#### 7.1 BaseController.java

```java
public class BaseController {

    protected <T> ResponseEntity<{ServiceName}ResponseDto<T>> mapSuccessResponse(T data) {
        return ResponseEntity.ok(new {ServiceName}ResponseDto<>(HttpStatus.OK.value(), "Success", data));
    }

    protected <T> ResponseEntity<{ServiceName}ResponseDto<T>> mapCreatedResponse(T data) {
        return ResponseEntity.status(HttpStatus.CREATED)
            .body(new {ServiceName}ResponseDto<>(HttpStatus.CREATED.value(), "Created", data));
    }

    protected <T> ResponseEntity<{ServiceName}ResponseDto<T>> mapErrorResponse(String message, HttpStatus status) {
        return ResponseEntity.status(status)
            .body(new {ServiceName}ResponseDto<>(status.value(), message, null));
    }
}
```

#### 7.2 Domain Controller

```java
@Slf4j
@RestController
@RequestMapping("/api/{domain-path}/")
@Tag(name = "{DomainController}", description = "Operations related to {DOMAIN_DESCRIPTION}")
public class {Domain}Controller extends BaseController {

    private final I{Domain}Service domainService;

    @Autowired
    public {Domain}Controller(I{Domain}Service domainService) {
        this.domainService = domainService;
    }

    @PostMapping(produces = APPLICATION_VND_{ORG}_{VERSION}_JSON, path = "resource")
    @Operation(summary = "Create Resource", description = "Creates a new resource")
    public Mono<ResponseEntity<{ServiceName}ResponseDto<{Domain}ResponseDto>>> create(
            @RequestBody {Domain}RequestDto requestDto) {
        return domainService.create(requestDto).map(this::mapSuccessResponse);
    }

    @GetMapping(produces = APPLICATION_VND_{ORG}_{VERSION}_JSON, path = "resources")
    @Operation(summary = "Get All Resources", description = "Retrieves all resources")
    public Mono<ResponseEntity<{ServiceName}ResponseDto<List<{Domain}ResponseDto>>>> getAll() {
        return domainService.getAll().collectList().map(this::mapSuccessResponse);
    }
}
```

> All controller methods must return `Mono<ResponseEntity<...>>`.

---

### 8. DATA LAYER

#### 8.1 R2DBC Entity

```java
@Table("{table_name}")
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class {Domain}Entity {
    @Id
    private UUID id;
    private String status;
    private String createdBy;
    private LocalDateTime createdAt;
    private String updatedBy;
    private LocalDateTime updatedAt;
    // Add domain-specific fields
}
```

> Use `UUID` primary keys. Include `createdAt`, `updatedAt`, `createdBy`, `updatedBy` audit fields on every entity.

#### 8.2 Activity Audit Entity

Always include an `ActivityAudit` entity with:
- `id` (UUID, `@Id`)
- `entityId` (String)
- `entityName` (String)
- `actionType` (String)
- `userId` (UUID)
- `updatedAt` (LocalDateTime)

#### 8.3 R2DBC Repository

```java
public interface {Domain}Repository extends ReactiveCrudRepository<{Domain}Entity, UUID> {
    Flux<{Domain}Entity> findByStatus(String status);
    Mono<{Domain}Entity> findByIdAndStatus(UUID id, String status);
}
```

---

### 9. DATABASE MIGRATIONS (LIQUIBASE)

#### 9.1 Master Changelog (`{service-name}-changelog.xml`)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
    xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
        http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-4.9.xsd">
    <includeAll path="db/changelog/changes/"/>
</databaseChangeLog>
```

#### 9.2 Initial SQL Script (`1.initial_database_creation.sql`)

Create the main domain table AND an `activity_audit` table. Use SQL Server (T-SQL) syntax:

```sql
-- Main domain table
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = '{table_name}')
BEGIN
    CREATE TABLE {table_name} (
        id                UNIQUEIDENTIFIER    NOT NULL DEFAULT NEWID() PRIMARY KEY,
        status            NVARCHAR(50)        NOT NULL,
        created_by        NVARCHAR(255)       NULL,
        created_at        DATETIME2           NULL DEFAULT GETUTCDATE(),
        updated_by        NVARCHAR(255)       NULL,
        updated_at        DATETIME2           NULL DEFAULT GETUTCDATE()
        -- Add domain-specific columns here
    );
END;

-- Activity audit table (always required)
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'activity_audit')
BEGIN
    CREATE TABLE activity_audit (
        id                UNIQUEIDENTIFIER    NOT NULL DEFAULT NEWID() PRIMARY KEY,
        entity_id         NVARCHAR(255)       NOT NULL,
        entity_name       NVARCHAR(255)       NOT NULL,
        action_type       NVARCHAR(100)       NOT NULL,
        user_id           UNIQUEIDENTIFIER    NOT NULL,
        updated_at        DATETIME2           NOT NULL DEFAULT GETUTCDATE()
    );
END;
```

> Use `IF NOT EXISTS` guards on all DDL statements. Prefix additional change files with sequential numbers: `2.add_columns.sql`, `3.alter_column.sql`, etc.

---

### 10. MAPSTRUCT MAPPERS

#### 10.1 Domain Mapper Interface

```java
@Mapper(
    nullValueCheckStrategy = NullValueCheckStrategy.ALWAYS,
    collectionMappingStrategy = CollectionMappingStrategy.TARGET_IMMUTABLE,
    unmappedTargetPolicy = ReportingPolicy.WARN,
    imports = {Arrays.class, Collections.class, StringUtils.class}
)
public interface I{Domain}Mapper {

    @Mapping(target = "responseId", source = "id")
    @Mapping(target = "responseStatus", source = "status")
    {Domain}ResponseDto map({Domain}Entity source);

    @Mapping(target = "id", ignore = true)
    @Mapping(target = "createdAt", expression = "java(LocalDateTime.now())")
    {Domain}Entity map({Domain}RequestDto source);
}
```

#### 10.2 Mapper Utility

Create a `{ServiceName}MapperUtil.java` with static helper methods that delegate to injected MapStruct mapper instances. Use `@Mapper(componentModel = "spring")` so mappers are Spring beans.

---

### 11. KAFKA NOTIFICATION PATTERN

#### 11.1 Properties Class (`{ServiceName}Properties.java`)

```java
@ConfigurationProperties(prefix = "com.{org-name}.{service-short-name}", ignoreUnknownFields = true)
@Getter @Setter
public class {ServiceName}Properties {
    private Kafka kafka;
    private Map<String, NotificationContent> notificationEventsContent;

    @Getter @Setter
    public static class Kafka {
        private KafkaProducerConfig emailNotificationProducerConfig;
    }

    @Getter @Setter
    public static class KafkaProducerConfig {
        private String topicName;
        private Integer maxRetries;
        private Long retryDelay;
    }

    @Getter @Setter
    public static class NotificationContent {
        private String eventType;
        private Boolean emailEnabled;
        private String messageTitle;
        private String messageBody;
    }
}
```

#### 11.2 Core Schema Notification Mapper (Static Utility)

```java
// CoreSchemaNotificationRequestMapper.java
// Final class, private constructor — all static methods
// buildCoreSchemaEmailNotificationRequest(eventType, emailAddresses, messageTitle, messageBody)
//   → returns EmcoCoreRequestDto with EventMetadata (UUID eventId, epoch eventTs, Source) + data list
```

#### 11.3 Kafka Publish Event Request Mapper (Spring Component)

```java
@Slf4j
@Component
@RequiredArgsConstructor
public class KafkaNotificationEventRequestMapper {

    private final {ServiceName}Properties properties;
    private final ObjectMapper objectMapper;

    // buildKafkaPublishEventRequest(event, requestDto, notificationContent)
    //   → reads topic name from properties
    //   → sets StringSerializer for key and value
    //   → builds list of OCECKafkaEventRequest<String, String>
    //   → returns OCECKafkaPublishEventRequest<String, String>
}
```

---

### 12. DTO PATTERNS

#### 12.1 Generic Response Wrapper

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class {ServiceName}ResponseDto<T> {
    private int status;
    private String message;
    private T data;
}
```

#### 12.2 Error Response

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class ErrorResponse {
    private int status;
    private String error;
    private String message;
    private LocalDateTime timestamp;
}
```

---

### 13. VALIDATION UTILITIES

#### 13.1 Input Validator (`{Domain}Validations.java`)

```java
public final class {Domain}Validations {
    private {Domain}Validations() {}

    // validateRequest(dto) → returns Mono<{Domain}RequestDto> or Mono.error(ResponseStatusException)
    // validateRequiredField(value, fieldName) → throws if blank
    // Use StringUtils.isBlank() for null/blank checks
    // Use Mono.error(new ResponseStatusException(HttpStatus.BAD_REQUEST, "...")) for reactive errors
}
```

#### 13.2 Common Utilities (`CommonUtils.java`)

```java
public final class CommonUtils {
    private CommonUtils() {}

    public static UUID parseStringToUUID(String value) { ... }
    public static LocalDateTime parseToLocalDateTime(String value) { ... }
    public static void logError(Logger log, String message, Throwable error) { ... }
    public static void logInfo(Logger log, String message, Object... args) { ... }
}
```

---

### 14. LOGGING CONFIGURATION

#### 14.1 `log/platform-core-logback.xml` (default)

```xml
<configuration>
    <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
    <root level="DEBUG"/>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder><pattern>${CONSOLE_LOG_PATTERN}</pattern></encoder>
    </appender>
    <logger name="org.springframework.web" level="INFO"><appender-ref ref="CONSOLE"/></logger>
    <logger name="org.springframework.data" level="DEBUG"><appender-ref ref="CONSOLE"/></logger>
    <logger name="org.springframework" level="INFO"><appender-ref ref="CONSOLE"/></logger>
    <logger name="io.netty" level="INFO"/>
    <logger name="{BASE_PACKAGE}" level="DEBUG"><appender-ref ref="CONSOLE"/></logger>
</configuration>
```

#### 14.2 `log/platform-core-logback-local.xml` (local dev)

Same as above but with `root level="DEBUG"` and all application loggers at DEBUG.

#### 14.3 `log/platform-core-logback-prod.xml` (production)

Same structure but with `root level="WARN"`, application loggers at `INFO`, and a rolling file appender.

---

### 15. DOCKERFILE

```dockerfile
FROM {BASE_IMAGE}  # e.g., openjdk17 Ubuntu 22 base
USER root

RUN groupadd -g 1999 {service}-grp && useradd -r -u 1999 -g root {service}-app-user
RUN mkdir /app

COPY target/*.jar /app/{artifact-id}-1.0-SNAPSHOT.jar

RUN chown {service}-app-user:{service}-grp /app -R

EXPOSE 8080
USER {service}-app-user
CMD ["java", "-jar", "/app/{artifact-id}-1.0-SNAPSHOT.jar"]
```

---

### 16. UNIT TESTS

Generate unit tests for all non-excluded packages. Minimum code coverage: **80%**.

#### 16.1 Test Framework Setup

- Use `@ExtendWith(MockitoExtension.class)` for unit tests
- Use `StepVerifier` for all reactive (`Mono`/`Flux`) assertions
- Use `@MockBean` / `@Mock` for all dependencies
- Use `@InjectMocks` for the class under test

#### 16.2 Controller Tests

```java
@ExtendWith(MockitoExtension.class)
class {Domain}ControllerTest {
    @Mock private I{Domain}Service domainService;
    @InjectMocks private {Domain}Controller controller;

    @Test
    void create_shouldReturnCreatedResponse_whenValidRequest() {
        // Arrange: mock service to return Mono.just(responseDto)
        // Act: call controller.create(requestDto)
        // Assert: StepVerifier.create(...).assertNext(response -> {
        //   assertNotNull(response);
        //   assertEquals(HttpStatus.OK, response.getStatusCode());
        // }).verifyComplete();
    }
}
```

#### 16.3 Service Tests

```java
@ExtendWith(MockitoExtension.class)
class {Domain}ServiceTest {
    @Mock private {Domain}Repository repository;
    @InjectMocks private {Domain}ServiceImpl service;

    @Test
    void create_shouldSaveEntity_whenValidInput() { ... }

    @Test
    void create_shouldReturnError_whenRepositorySaveFails() { ... }
}
```

#### 16.4 Utility / Mapper Tests

Write tests for all validation methods, all mapper `map()` method signatures, and all utility methods in `CommonUtils`.

---

### 17. ACTIONSFILE (CI/CD Pipeline Configs)

Create a file for each environment in `Actionsfile/` directory (no file extension):

```
# Actionsfile/{env}
SERVICE_NAME={ARTIFACT_ID}
ENVIRONMENT={env}
NAMESPACE={k8s-namespace}
IMAGE_TAG=latest
REPLICAS=2
RESOURCE_CPU_REQUEST=500m
RESOURCE_CPU_LIMIT=1000m
RESOURCE_MEMORY_REQUEST=512Mi
RESOURCE_MEMORY_LIMIT=1024Mi
```

Environments: `dev`, `qa1`, `qa2`, `perf1`, `stage`, `prod`

---

### 18. CHECKLIST — VERIFY BEFORE FINALIZING

Before submitting the generated code, verify every item below is present and correct:

- [ ] `WebApplicationType.REACTIVE` in main class
- [ ] All service methods return `Mono<T>` or `Flux<T>` (no blocking calls)
- [ ] `SecurityWebFilterChain` bean that disables security filter
- [ ] `@PostConstruct` build info logger in main config
- [ ] `TimedAspect` bean in `MetricsConfig`
- [ ] Pass-through `WebClientCustomizer` in `ReactiveWebServerConfig`
- [ ] `LoggingAspect` with `@Around` on both controller and service pointcuts
- [ ] `AuditAspect` with `@AfterReturning` on `@Auditable` annotation
- [ ] `UserContextWebFilter` populating Reactor context from HTTP headers
- [ ] `GlobalExceptionHandler` with all 5 exception types handled
- [ ] Liquibase configured with JDBC URL (not R2DBC) for migrations
- [ ] `activity_audit` table in initial SQL script
- [ ] R2DBC connection pool configured with validation query `SELECT 1`
- [ ] MapStruct mappers use `componentModel = "spring"` (via compiler arg)
- [ ] `@ConfigurationProperties` for Kafka + notification content
- [ ] Generic `{ServiceName}ResponseDto<T>` wrapper used in all controller responses
- [ ] JaCoCo minimum 80% instruction coverage enforced in `pom.xml`
- [ ] Surefire fork count = 2, run order = alphabetical
- [ ] Dockerfile creates dedicated non-root user and group
- [ ] Logback configs for default, local, and prod profiles
- [ ] `bootstrap.yml` with `searchLocations: classpath:/config`
- [ ] All tests use `StepVerifier` for reactive assertions
- [ ] `sonar.coverage.exclusions` matches JaCoCo exclusions in `pom.xml`
- [ ] Prometheus metrics endpoint enabled
- [ ] `springdoc` Swagger UI enabled at `/swagger-ui.html`
- [ ] Spring Cloud LoadBalancer configured (Ribbon disabled)
- [ ] OAuth2 client credentials configured with `client_secret_post`

---

## 📝 Placeholder Reference Table

| Placeholder | Example Value | Description |
|---|---|---|
| `{SERVICE_NAME}` | `order-management-service` | Kebab-case service name |
| `{SERVICE_NAME_UPPER}` | `ORDER_MANAGEMENT_SERVICE` | Upper-snake-case service name |
| `{GROUP_ID}` | `com.mycompany.team` | Maven group ID |
| `{ARTIFACT_ID}` | `order-management-service` | Maven artifact ID |
| `{BASE_PACKAGE}` | `com.mycompany.team.order` | Java base package |
| `{BASE_PACKAGE_PATH}` | `com/mycompany/team/order` | File system path of base package |
| `{TOP_LEVEL_SCAN_PACKAGE}` | `com.mycompany` | Top-level package for `@ComponentScan` |
| `{MainClassName}` | `OrderManagementServiceApplication` | Spring Boot main class name |
| `{ServiceName}` | `OrderManagement` | PascalCase service name |
| `{Domain}` | `Order` | PascalCase domain entity name |
| `{domain-path}` | `orders` | URL path segment for REST endpoints |
| `{table_name}` | `order_management` | SQL table name |
| `{DOMAIN_DESCRIPTION}` | `order lifecycle management` | Business description |
| `{org-name}` | `mycompany` | Organization name for config prefix |
| `{service-short-name}` | `order` | Short name for config prefix |
| `{downstream-service-name}` | `inventory-service` | Name of external downstream service |
| `{downstream-service-base-url}` | `https://api.mycompany.com` | Base URL of downstream service |
| `{tenant-id}` | `b7f604a0-...` | Azure AD tenant ID |
| `{KAFKA_TOPIC_NAME}` | `ENTERPRISE_EMAIL_NOTIFICATION` | Kafka topic for notifications |
| `{event-key-1}` | `create-order` | First notification event key |
| `{event-key-2}` | `update-order-status` | Second notification event key |
| `{EventType1NotificationEvent}` | `CreateOrderNotificationEvent` | Event type class name |
| `{BASE_IMAGE}` | `openjdk17:ubuntu22` | Docker base image |

---

## 🗂️ Architecture Summary Diagram

```
┌────────────────────────────────────────────────────────────────┐
│                    HTTP Request                                 │
└─────────────────────────┬──────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│              UserContextWebFilter                               │
│   (Reads X-User-Id + x-correlation-id → Reactor Context)       │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│  LoggingAspect (@Around)    MetricsAspect (@Timed)              │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│              {Domain}Controller extends BaseController          │
│         (@RestController, @RequestMapping, Swagger @Tag)        │
└─────────────────────────┬───────────────────────────────────────┘
                          │  Mono<T> / Flux<T>
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│         {Domain}Validations (reactive input validation)         │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│              I{Domain}Service / {Domain}ServiceImpl             │
│           (Business logic, @Transactional, @Auditable)          │
│                                                                 │
│   ┌───────────────────────────────────────────────────────┐    │
│   │           AuditAspect (@AfterReturning)               │    │
│   │    ActivityAuditService → ActivityAuditRepository     │    │
│   └───────────────────────────────────────────────────────┘    │
└──────┬───────────────────────┬──────────────────────────────────┘
       │                       │
       ▼                       ▼
┌──────────────┐   ┌────────────────────────────────────────────┐
│  R2DBC       │   │  Kafka Notification                        │
│  Repositories│   │  KafkaNotificationEventRequestMapper       │
│  (Reactive   │   │  → OCECKafkaPublishEventRequest            │
│   R2DBC)     │   │  → Downstream Kafka Topic                  │
└──────┬───────┘   └────────────────────────────────────────────┘
       │
       ▼
┌──────────────┐
│  SQL Server  │
│  (R2DBC Pool)│
│  + Liquibase │
│  Migrations  │
└──────────────┘
```

---

*This blueprint was generated from the `{app}-service` reference project on April 14, 2026.*
*Replace all `{PLACEHOLDER}` values before using this prompt.*