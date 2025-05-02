
# 🧩 RESTful 컨트롤러와 비동기 요청 실습

## ✅ `@RestController` 개념 및 설정

---

## 📘 기본 개념

### 🌐 `@RestController`

- `@Controller` + `@ResponseBody` 결합된 어노테이션
    
- 모든 메서드의 반환 값을 JSON, XML 등 **데이터 객체 자체로 반환**
    
- Spring MVC 기반 RESTful API 설계에 필수
    
- 주로 프론트엔드와 통신하는 **백엔드 API 서버**로 동작
    

### 📌 REST란?

- **REpresentational State Transfer**의 약자
    
- HTTP를 기반으로 자원을 URI로 표현하고, HTTP 메서드(GET, POST 등)를 통해 상태를 주고받는 아키텍처 스타일
    
- 이 개념을 구현한 서비스는 **RESTful**하다고 표현
    

---

## ⚙️ REST 컨트롤러 테스트를 위한 기본 의존성

```xml
<!-- jackson: JSON/XML 직렬화용 -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.19.0</version>
</dependency>

<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.19.0</version>
</dependency>

<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.19.0</version>
</dependency>
```

---

## 🔄 비동기 요청 클라이언트 종류 (JavaScript)

|도구|등장 연도|특징|
|---|---|---|
|**XMLHttpRequest (XHR)**|1999|가장 오래된 비동기 기술, 복잡하고 콜백 지옥 발생|
|**Promise**|2015 (ES6)|콜백을 체이닝으로 처리. 비동기 흐름 제어|
|**Fetch API**|2015 (ES6)|Promise 기반 네트워크 API. 코드 간결|
|**Axios**|2014|HTTP 클라이언트 라이브러리. 인터셉터, JSON 자동 직렬화 지원. 프론트-백 통신에 많이 사용됨|

---

## 📝 요약

- `@RestController`는 REST API 서버를 만드는 핵심 어노테이션
    
- JSON/XML 등의 데이터 포맷 반환이 기본
    
- 프론트와 연동 시 `axios`, `fetch`, `XHR` 등의 다양한 비동기 방식 사용 가능
    
- 이를 위해 Jackson 관련 의존성이 필요
    

---
