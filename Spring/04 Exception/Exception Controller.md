# ⚠️ 예외 처리 컨트롤러 - `ExceptionTestController`

## 📦 전체 코드

```java
package com.example.app.controller;

@Controller
@Slf4j
@RequestMapping("/except")
public class ExceptionTestController {
	
	@ExceptionHandler(Exception.class)
	public String exceptionHandler(Exception e, Model model) {
		log.error("error : " + e);
		return "except/error";
	}
	
	@GetMapping("/page01")
	public void ex1() throws FileNotFoundException {
		log.info("GET /except/page01");
		throw new FileNotFoundException("파일을 찾을 수 없습니다.");
	}
	
	@GetMapping("/page02/{num}/{div}")
	public String ex2(
			@PathVariable("num") int num,
			@PathVariable("div") int div,
			Model model
	) throws ArithmeticException {
		log.info("GET /except/page02..." + (num / div));
		model.addAttribute("result", (num / div));
		return "except/page02";
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Controller`: 이 클래스가 스프링 MVC에서 컨트롤러 역할을 함을 명시
    
- `@Slf4j`: 로그 출력을 위한 Lombok 어노테이션, `log.info(...)` 또는 `log.error(...)` 사용 가능
    
- `@RequestMapping("/except")`: 해당 클래스의 모든 요청 URI는 `/except`로 시작
    

---

### ✅ 예외 처리 핸들러

- `@ExceptionHandler(Exception.class)`:
    
    - 이 컨트롤러에서 발생하는 모든 예외(`Exception`)를 처리
        
    - 예외 정보는 로그에 출력하고, 에러 페이지 `except/error.jsp`로 포워딩
        
    - 현재는 모든 예외를 `Exception.class` 하나로 처리 중
        
    - 주석 처리된 부분처럼 구체적인 예외(`FileNotFoundException`, `ArithmeticException`)에 대해 개별 핸들러 정의 가능
        

---

### ✅ 주요 메서드 설명

- `ex1()`
    
    - `@GetMapping("/page01")`
        
    - 강제로 `FileNotFoundException` 발생 → 에러 페이지로 이동
        
    - 테스트용 예외 발생 메서드
        
- `ex2(int num, int div)`
    
    - `@GetMapping("/page02/{num}/{div}")`
        
    - URL 경로에서 `num`, `div` 값을 받아 나눗셈 실행
        
    - `div`가 0일 경우 `ArithmeticException` 발생
        
    - 정상적으로 계산되면 `result` 값을 모델에 저장하고 `except/page02.jsp`로 이동
        

---

## 📌 요약

- 이 컨트롤러는 예외 처리 학습을 위한 테스트용
    
- `/page01` 요청은 파일 예외, `/page02/{num}/{div}`는 나눗셈 예외를 각각 유발
    
- `@ExceptionHandler(Exception.class)`를 통해 공통 예외 처리
    
- 실제 서비스에서는 예외 유형별로 별도의 핸들러를 작성하는 것이 바람직함