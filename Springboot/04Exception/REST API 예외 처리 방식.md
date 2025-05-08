
---

# 📡 REST API 예외 처리 방식 정리

## ✅ 1. `@ResponseStatus` 사용

### 📌 예외 클래스 정의

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String msg) {
        super(msg);
    }
}
```

- 예외 발생 시 자동으로 `HTTP 404` 상태코드 반환
    

---

### 📌 컨트롤러에서 예외 발생

```java
@RestController
@RequestMapping("/api/memo")
public class MemoRestController {

    @GetMapping("/{id}")
    public MemoDto getMemo(@PathVariable Long id) {
        MemoDto memo = memoService.findById(id);
        if (memo == null) {
            throw new ResourceNotFoundException("해당 메모를 찾을 수 없습니다.");
        }
        return memo;
    }
}
```

- `404 Not Found`와 함께 메시지 반환됨
    

---

## ✅ 2. `@RestControllerAdvice` + `ResponseEntity`

### 📌 전역 예외 처리 클래스

```java
@RestControllerAdvice
public class ApiExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                             .body("에러: " + ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneral(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                             .body("알 수 없는 오류가 발생했습니다.");
    }
}
```

- `@RestControllerAdvice`는 JSON 응답 전용 예외 처리
    
- `ResponseEntity`를 사용하면 **상태코드 + 메시지**를 직접 조절 가능
    

---

## ✅ 3. 예외 응답 포맷을 객체로 반환 (선택적)

```java
@Getter
@AllArgsConstructor
public class ErrorResponse {
    private int status;
    private String message;
    private String timestamp;
}
```

```java
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
    ErrorResponse response = new ErrorResponse(
        404, ex.getMessage(), LocalDateTime.now().toString());
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body(response);
}
```

- JSON 응답 구조를 통일할 수 있음
    

---

## 📌 요약

|항목|설명|
|---|---|
|`@ResponseStatus`|예외에 HTTP 상태코드 지정|
|`@RestControllerAdvice`|REST API 전용 예외 처리|
|`ResponseEntity`|상태코드 + 응답 메시지 자유 구성|
|객체형 에러 응답|통일된 JSON 구조 반환 시 사용|

---
