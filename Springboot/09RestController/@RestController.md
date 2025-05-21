
# 📡 `@RestController` 

## ✅ 기본 개념

### 🔷 정의

- `@RestController`는 `@Controller` + `@ResponseBody`가 결합된 단축 어노테이션
    
- View(JSP, Thymeleaf 등)를 반환하지 않고, **HTTP 응답 바디에 직접 데이터(JSON/XML)**를 담아서 반환하는 데 특화됨
    

### 🔷 주요 사용처

- RESTful API 설계 시
    
- 프론트엔드(React, Vue 등) 또는 외부 클라이언트와 통신할 때
    
- Postman, Axios, Fetch API 등에서 직접 호출하는 API 서버 구현
    

---

## 🧱 핵심 구조

```java
@RestController
@RequestMapping("/api")
public class SampleRestController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello REST";
    }
}
```

- `@RestController`: 해당 클래스의 모든 메서드는 응답 바디로 직접 반환됨
    
- `@RequestMapping`: 공통 URI prefix
    
- `@GetMapping`, `@PostMapping` 등: 각각 HTTP Method를 의미
    

---

## 🚦 주요 어노테이션 정리

|어노테이션|설명|
|---|---|
|`@RestController`|JSON 응답 전용 컨트롤러 선언|
|`@RequestMapping`|URI 및 공통 설정|
|`@GetMapping`|GET 요청 (조회)|
|`@PostMapping`|POST 요청 (등록)|
|`@PutMapping`|PUT 요청 (전체 수정)|
|`@PatchMapping`|PATCH 요청 (부분 수정)|
|`@DeleteMapping`|DELETE 요청 (삭제)|
|`@RequestBody`|JSON → Java 객체로 역직렬화|
|`@ResponseBody`|Java 객체 → JSON으로 직렬화|
|`@PathVariable`|URI 경로 변수 바인딩|
|`@RequestParam`|쿼리스트링 파라미터 바인딩|

---

## 🧪 실습 예제 (Postman 테스트용)

### ✅ JSON 요청을 받아 처리

```java
@PostMapping("/join")
public ResponseEntity<?> join(@RequestBody @Valid JoinRequestDto dto, BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        String errorMessage = bindingResult.getFieldError().getDefaultMessage();
        return ResponseEntity.badRequest().body(Collections.singletonMap("error", errorMessage));
    }

    userService.save(dto);
    return ResponseEntity.ok(Collections.singletonMap("message", "회원가입 완료"));
}
```

- `@RequestBody`: 클라이언트에서 JSON으로 보낸 데이터를 DTO로 바인딩
    
- `@Valid`: 유효성 검사 적용
    
- `ResponseEntity`: HTTP 상태 코드 및 응답 메시지를 함께 보낼 수 있음
    

---

## 🔄 응답 구조 예시

### 📤 클라이언트 → 서버 (POST)

```json
{
  "userid": "user01",
  "password": "1234",
  "username": "홍길동"
}
```

### 📥 서버 → 클라이언트 (ResponseEntity)

```json
{
  "message": "회원가입 완료"
}
```

또는 유효성 실패 시:

```json
{
  "error": "아이디는 필수입니다."
}
```

---

## 🔐 @RestController vs @Controller 비교

|구분|@RestController|@Controller|
|---|---|---|
|반환값|JSON (또는 XML)|View (JSP 등)|
|사용 용도|REST API 전용|서버사이드 렌더링|
|대표 사용법|`ResponseEntity`, `@RequestBody`|`Model`, `@RequestParam`|
|예시|`return ResponseEntity.ok(user);`|`return "userView";`|

---

## 📌 실무 팁

- 프론트와 백을 분리하는 구조(BE: Spring Boot, FE: React 등)에서는 **무조건 @RestController 사용**
    
- `ResponseEntity`로 상태코드, 헤더, 바디를 명시적으로 반환하는 것이 실무에서는 표준
    
- API 문서 자동화를 위해 Swagger (`springdoc-openapi`)를 함께 사용하면 편리
    

---

## ✅ 요약

- `@RestController`는 REST API 구축에 최적화된 어노테이션
    
- View 반환 없이 JSON 데이터 직렬화를 기본으로 함
    
- `@RequestBody`, `@ResponseBody`, `@PathVariable`, `@RequestParam` 등과 조합해 다양한 요청 처리 가능
    
- 프론트와의 통신, 모바일 앱 연동, 공공 API 제공 등 다양한 분야에서 사용됨
    

---
