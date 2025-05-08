
---

# 🔍 Spring AOP 정리 (STS3 + Spring 5.0.7.RELEASE)

## 1️⃣ AOP란?

- **Aspect-Oriented Programming** (관점 지향 프로그래밍)
    
- 핵심 비즈니스 로직과 공통 관심사(로깅, 보안, 트랜잭션 등)를 분리해서 모듈화하는 프로그래밍 기법
    
- 중복 제거, 유지보수 향상, 관심사 분리에 도움
    

---

## 2️⃣ 핵심 용어

|용어|설명|
|---|---|
|Aspect|공통 기능을 모듈화한 객체|
|Advice|실제로 수행되는 공통 기능 로직 (`@Before`, `@After` 등)|
|JoinPoint|공통 기능이 삽입될 수 있는 지점 (메서드 실행 등)|
|Pointcut|JoinPoint 중에 Advice가 적용될 위치를 지정하는 조건|
|Weaving|Advice를 실제 객체에 적용하는 과정|

---

## 3️⃣ Spring AOP 특징

- **프록시 기반 AOP**
    
- **런타임 Weaving** 방식
    
- **메서드 실행 시점** 중심 (`Method Execution JoinPoint`)
    
- JDK Proxy 또는 CGLIB 기반
    

---

## 4️⃣ AOP 설정

### Maven 의존성 (pom.xml)

```xml
<!-- Spring AOP -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.0.7.RELEASE</version>
</dependency>

<!-- AspectJ -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.7</version>
</dependency>
```

### XML 설정 (root-context.xml)

```xml
<aop:aspectj-autoproxy />
```

### Java Config

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
}
```

---

## 5️⃣ 실습 예제

### ✅ Aspect 클래스 - `LogAspect.java`

```java
@Aspect
@Component
public class LogAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void beforeLog() {
        System.out.println("메서드 실행 전에 로그 기록");
    }

    @AfterReturning("execution(* com.example.service.*.*(..))")
    public void afterReturningLog() {
        System.out.println("메서드 정상 실행 후 로그 기록");
    }

    @AfterThrowing("execution(* com.example.service.*.*(..))")
    public void afterThrowingLog() {
        System.out.println("메서드 예외 발생 후 로그 기록");
    }
}
```

### ✅ 비즈니스 로직 클래스 - `SampleService.java`

```java
@Service
public class SampleService {

    public void doSomething() {
        System.out.println("비즈니스 로직 실행");
    }
}
```

### ✅ 테스트 클래스 - `AopTest.java`

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
public class AopTest {

    @Autowired
    private SampleService sampleService;

    @Test
    public void testAop() {
        sampleService.doSomething();
    }
}
```

---

## 6️⃣ Advice 종류 요약

|종류|설명|
|---|---|
|`@Before`|메서드 실행 **이전**에 수행|
|`@AfterReturning`|메서드 **정상 종료 후** 수행|
|`@AfterThrowing`|메서드 **예외 발생 후** 수행|
|`@After`|메서드 실행 후 **무조건** 수행|
|`@Around`|메서드 **전/후/예외 포함 전체 제어**|

---

## 📌 요약

- Spring AOP는 **프록시 기반**으로 작동하며, 핵심 비즈니스 로직과 공통 관심사를 분리해 관리
    
- `@Aspect` 클래스를 통해 공통 로직을 Advice로 작성하고, 원하는 위치에 적용 (`execution()` Pointcut)
    
- 주로 사용하는 공통 기능: **로깅, 트랜잭션, 보안, 예외 처리**
    
- `@EnableAspectJAutoProxy` 또는 `<aop:aspectj-autoproxy />`를 통해 활성화
    

---
