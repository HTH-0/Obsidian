
---

# 🧱 Custom Bean 등록 방식

## 1. 설정 파일에 등록

### 📌 (1) XML 설정

- 가장 오래된 방식. `applicationContext.xml` 같은 설정 파일에 `<bean>` 태그로 등록.
    
- **형식**:
    

```xml
<bean id="personDto1" class="com.example.app.domain.dto.PersonDto">
    <constructor-arg name="username" value="홍길동" />
    <constructor-arg name="age" value="11" />
    <constructor-arg name="addr" value="창원" />
</bean>
```

- 장점: XML만 보면 전체 빈 구성을 파악 가능
    
- 단점: 유지보수 불편, 타입 안전성 부족, 코드와 설정이 분리됨
    

---

### 📌 (2) properties / yml / yaml 설정

- 주로 Spring Boot에서 외부설정 값들을 읽기 위해 사용
    
- `Bean 등록` 자체보다는 **설정값 주입**에 많이 쓰임
    
- 예시: `application.yml`
    

```yaml
my.bean:
  username: 홍길동
  age: 11
  addr: 창원
```

- @ConfigurationProperties 또는 @Value를 통해 Java 객체에 매핑
    

---

## 2. JavaConfig 방식 (`@Configuration` 사용)

### 📌 (1) 수동 등록 방식

- Java 파일에 직접 `@Bean` 메서드를 정의하여 객체 생성 및 등록
    
- 예시:
    

```java
@Configuration
public class AppConfig {

    @Bean
    public PersonDto personDto1() {
        return new PersonDto("홍길동", 11, "창원");
    }
}
```

- 장점:
    
    - 코드 기반이므로 **타입 안전성** 보장
        
    - IDE 자동완성, 리팩토링 가능
        
    - 테스트와 관리가 쉬움
        
- 단점: XML 기반보다 구조적으로는 덜 직관적일 수 있음
    

---

### 📌 (2) 자동 등록 방식 (`@Component`, `@Service`, `@Repository`, `@Controller`)

- 컴포넌트 스캔을 통해 클래스 위에 어노테이션을 붙여 자동 등록
    

```java
@Component
public class MyBean { ... }
```

- 그리고 `@ComponentScan` 또는 XML 설정으로 자동 탐지 경로 지정
    

```java
@ComponentScan(basePackages = "com.example.app")
```

- 장점: 코드에 선언만 하면 자동으로 등록되어 간결
    
- 단점: **어떤 빈이 등록되었는지 명시성이 떨어짐**
    

---

# 🔁 정리 비교

|구분|방식|특징|사용 시기|
|---|---|---|---|
|XML 설정|`<bean>`|명시적, 구식 방식|레거시 프로젝트|
|JavaConfig 수동|`@Configuration + @Bean`|타입 안전, 명확함|대부분의 Spring 프로젝트|
|JavaConfig 자동|`@Component 등` + `@ComponentScan`|간편, 유지보수 편리|Spring Boot 주류 방식|
|yml/properties|설정값 주입용|Bean 직접 생성용은 아님|설정값 외부화 필요 시|

---
