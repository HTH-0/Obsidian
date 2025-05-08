
---

# 🚀 Spring Boot 프로젝트 초기 구성 (01_INIT)

## ✅ 프로젝트 생성

- [start.spring.io](https://start.spring.io/) 에서 생성
    
    - Gradle Project
        
    - Java 17+
        
    - Spring Boot 3.x 이상
        
    - Dependencies: `Spring Web`, `Lombok`
        


---

## 🛠️ IntelliJ 설정 (기본 세팅)

1. Gradle 프로젝트 Import 시
    
    - **"Use auto-import"** 체크
        
    - **"IntelliJ IDEA로 Gradle 실행"** 권장
        
2. Lombok 활성화
    
    - `File → Settings → Build, Execution, Deployment → Compiler → Annotation Processors`  
        → Enable annotation processing 체크
        
3. `build.gradle` 동기화
    
    - 우측 Gradle 탭에서 🔄 버튼 클릭 (또는 우클릭 → Reimport)
        


---

## 📦 기본 프로젝트 구조

```
src
├── main
│   ├── java
│   │   └── com.example.init
│   │       └── InitApplication.java   // main class
│   └── resources
│       ├── application.properties     // 설정파일
│       └── static / templates         // 정적 리소스 및 템플릿
└── test
```

---

## 📄 InitApplication.java

```java
package com.example.init;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class InitApplication {

    public static void main(String[] args) {
        SpringApplication.run(InitApplication.class, args);
    }
}
```

- `@SpringBootApplication` = `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`
    
- 프로젝트의 부트스트랩(시작점) 역할 수행
    

---

## ⚙️ application.properties

```properties
# 서버 포트 변경
server.port=8081

# JSP 사용 시 view 경로 설정
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

# UTF-8 설정
spring.servlet.encoding.charset=UTF-8
spring.servlet.encoding.enabled=true
spring.servlet.encoding.force=true
```

---

## 📝 build.gradle

```groovy
plugins {
    id 'org.springframework.boot' version '3.x.x'
    id 'io.spring.dependency-management' version '1.1.3'
    id 'java'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

java {
    sourceCompatibility = '17'
}
```

- `spring-boot-starter-web`: REST API, MVC 기본 기능 포함
    
- `lombok`: @Getter, @Setter 등 코드 줄이기
    
- `spring-boot-starter-test`: JUnit 기반 테스트 지원
    

---

## 📌 요약

| 항목       | 설명                                       |
| -------- | ---------------------------------------- |
| 프로젝트 생성  | start.spring.io에서 생성                     |
| 실행 클래스   | `InitApplication.java`                   |
| 설정 파일    | `application.properties`, `build.gradle` |
| 포트 설정    | 기본 8080 → 예제에선 8090로 변경                  |
| 뷰 설정     | JSP 기반 ViewResolver 설정 포함                |
| 로깅 및 테스트 | 기본 Web + Lombok + Test 구조 포함             |

---
