# 🌐 전역 예외 처리기 - `GlobalExceptionHandler`

## 📦 전체 코드

```java
package com.example.app.controller.exception;

@ControllerAdvice
@Slf4j
public class GlobalExceptionHandler {
	@ExceptionHandler(Exception.class)
	public String AllExceptionHandler(Exception e, Model model) {
		log.info("GlobalExceptionHandler's error : " + e);
		model.addAttribute("ex", e);
		return "global_error";
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@ControllerAdvice`
    
    - 전역 예외 처리용 클래스 지정
        
    - 프로젝트 내의 모든 컨트롤러에서 발생한 예외를 공통으로 처리할 수 있음
        
    - 컨트롤러마다 중복되는 `@ExceptionHandler` 코드를 줄이고 통합 관리 가능
        
- `@Slf4j`
    
    - Lombok을 통해 자동으로 `log` 객체 생성
        
    - 예외 로그를 출력하기 위해 사용 (`log.info(...)`)
        

---

### ✅ 예외 처리 메서드 설명

- `@ExceptionHandler(Exception.class)`
    
    - 프로젝트 전반에서 발생하는 모든 `Exception` 클래스 또는 그 하위 예외를 처리
        
    - 메서드 인자로 예외 객체(`Exception e`)와 모델 객체(`Model model`)를 받음
        
- `model.addAttribute("ex", e)`
    
    - 발생한 예외 객체를 모델에 담아 JSP 등 뷰 페이지에서 출력 가능하도록 전달
        
- `return "global_error"`
    
    - 예외 발생 시 보여줄 에러 페이지의 이름 (`/WEB-INF/views/global_error.jsp`)
        

---

## 📌 요약

- 이 클래스는 **전역 예외 처리 전담** 클래스
    
- `@ControllerAdvice` + `@ExceptionHandler` 조합으로 전 컨트롤러 예외를 포착해 처리
    
- 모든 예외를 잡아 `"global_error"` 뷰로 연결
    
- 예외 정보는 `Model`을 통해 뷰에 전달되며, 디버깅 및 사용자 안내에 활용 가능함
    

> 🔎 개별 컨트롤러에 예외 핸들러가 정의되어 있으면 **전역 처리기보다 우선 적용됨**  
> 따라서 전역 예외는 **마지막 안전망** 역할로 설계하는 것이 좋음.