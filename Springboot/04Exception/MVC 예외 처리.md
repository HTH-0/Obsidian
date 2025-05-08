
---

# 🚨 Spring MVC 예외 처리 실습 정리

## ✅ 예외 처리 구조

- **목표**: 컨트롤러 실행 중 발생하는 예외를 `@ExceptionHandler` 또는 `@ControllerAdvice`를 통해 처리하고, 사용자에게 오류 페이지 제공
    

---

## 📦 주요 클래스 코드

### 1. `GlobalExceptionHandler.java`

```java
@ControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public String allExceptionHandler(Exception e, Model model) {
        log.info("GlobalExceptionHandler's error : " + e);
        model.addAttribute("ex", e);
        return "global_error";
    }
}
```

- `@ControllerAdvice`: 모든 컨트롤러에서 발생한 예외를 잡아주는 전역 처리 클래스
    
- `@ExceptionHandler(Exception.class)`: 모든 종류의 예외 처리
    
- 예외 객체 `e`를 `model`에 담아 `"global_error.jsp"`로 전달
    

---

### 2. `ExceptionTestController.java`

```java
@Controller
@Slf4j
@RequestMapping("/except")
public class ExceptionTestController {

    @GetMapping("/ex")
    public void ex1_1() throws FileNotFoundException {
        throw new FileNotFoundException("파일을 찾을 수 없습니다.");
    }

    @GetMapping("/page01")
    public void ex1() throws FileNotFoundException {
        throw new FileNotFoundException("파일을 찾을 수 없습니다.");
    }

    @GetMapping("/page02/{num}/{div}")
    public String ex2(@PathVariable("num") int num,
                      @PathVariable("div") int div,
                      Model model) throws ArithmeticException {
        model.addAttribute("result", (num / div));
        return "except/page02";
    }

    // 컨트롤러 내부 @ExceptionHandler는 현재 주석 처리 상태
}
```

- `/except/ex` → `FileNotFoundException` 강제 발생
    
- `/except/page02/{num}/{div}` → `0으로 나누기` 시 `ArithmeticException` 발생
    

---

## 🧾 JSP 예외 출력 예시 (`global_error.jsp`)

```jsp
<%@ page contentType="text/html; charset=UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>예외 발생</title>
</head>
<body>
    <h1>⚠ 예외가 발생했습니다.</h1>
    <p>${ex}</p>
</body>
</html>
```

- `model.addAttribute("ex", e)`로 전달된 예외 객체를 출력
    

---

## 🔍 정리 요약

|구성요소|설명|
|---|---|
|`@ControllerAdvice`|전역 예외 처리 클래스|
|`@ExceptionHandler`|예외 유형별 메서드 처리|
|`Model`|예외 정보를 JSP로 전달|
|`global_error.jsp`|예외 메시지 사용자에게 출력|
|컨트롤러 내 핸들러|현재 주석 처리됨 → 전역 처리 우선 적용됨|

---
