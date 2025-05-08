
---

# 🔄 Spring 예외 처리 - View + REST 응답 혼합 구조

## ✅ 1. 공통 전역 처리 분리 전략

|구분|처리 방식|어노테이션|
|---|---|---|
|화면(JSP) 요청|`Model`, `return "view"`|`@ControllerAdvice`|
|API 요청|JSON 응답|`@RestControllerAdvice`|

---

## ✅ 2. View 전용 예외 처리 (`@ControllerAdvice`)

```java
@ControllerAdvice
@Slf4j
public class GlobalViewExceptionHandler {

    @ExceptionHandler(Exception.class)
    public String viewException(Exception ex, Model model) {
        log.warn("View 예외 처리: {}", ex.getMessage());
        model.addAttribute("ex", ex);
        return "global_error"; // JSP로 이동
    }
}
```

- `Model`을 이용해 JSP에 예외 메시지 전달
    
- `return`은 뷰 이름 (JSP 경로)
    

---

## ✅ 3. API 전용 예외 처리 (`@RestControllerAdvice`)

```java
@RestControllerAdvice
public class GlobalRestExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String, Object>> apiException(Exception ex) {
        Map<String, Object> error = new HashMap<>();
        error.put("status", 500);
        error.put("message", ex.getMessage());
        error.put("time", LocalDateTime.now().toString());

        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
```

- JSON 포맷으로 에러 응답 반환
    
- REST 컨트롤러 전용 예외 핸들링 담당
    

---

## ✅ 4. 실무에서 적용 구조 (권장 방식)

```plaintext
src/main/java
├─ controller/      → View용 컨트롤러 (@Controller)
├─ api/             → API 전용 컨트롤러 (@RestController)
├─ advice/
│   ├─ GlobalViewExceptionHandler.java
│   └─ GlobalRestExceptionHandler.java
```

- **View와 API를 패키지로 나누고**, 예외 처리도 각각 분리함
    
- 응답 방식이 섞여 있는 경우에도 안전하게 대응 가능
    

---

## ✅ 5. Postman 등 API 테스트 시 확인

### 요청

```http
GET /api/memo/999
Accept: application/json
```

### 응답

```json
{
  "status": 404,
  "message": "해당 메모를 찾을 수 없습니다.",
  "time": "2025-05-08T14:22:31"
}
```

---

## 📌 요약

|구성요소|설명|
|---|---|
|`@ControllerAdvice`|JSP 기반 예외 처리 (Model, View)|
|`@RestControllerAdvice`|REST API 예외 처리 (JSON 응답)|
|패키지 분리|View / API 구분 → 유지보수 향상|
|ResponseEntity|API 응답 형식 자유롭게 구성 가능|

---
