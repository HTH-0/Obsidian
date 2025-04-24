# 🧾 MemoController와 WebDataBinder 커스텀 설정

## 📦 전체 코드

```java

@Controller
@Slf4j
@RequestMapping("/memo")
public class MemoController {

	@InitBinder
	public void dataBinder(WebDataBinder webDataBinder) {
		log.info("MemoController's DataBinder ... " + webDataBinder);
		webDataBinder.registerCustomEditor(LocalDate.class, "dateTest", new DateTestEditor());
	}

	@GetMapping("/add")
	public void add_get() {
		log.info("GET /memo/add ...");
	}

	@PostMapping("/add")
	public void add_post(@Valid MemoDto dto, BindingResult bindingResult, Model model) {
		log.info("POST /memo/add ..." + dto);
		if (bindingResult.hasErrors()) {
			for (FieldError error : bindingResult.getFieldErrors()) {
				log.info("Error field : " + error.getField() + " Error Msg : " + error.getDefaultMessage());
				model.addAttribute(error.getField(), error.getDefaultMessage());
			}
		}
	}

	private static class DateTestEditor extends PropertyEditorSupport {
		@Override
		public void setAsText(String dateTest) throws IllegalArgumentException {
			log.info("DataTestEditor's setAsText invoke..." + dateTest);
			LocalDate date = null;
			if (dateTest.isEmpty()) {
				date = LocalDate.now();
			} else {
				dateTest = dateTest.replaceAll("#", "-");
				date = LocalDate.parse(dateTest, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
			}
			setValue(date);
		}
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Controller`: 해당 클래스가 Spring MVC의 컨트롤러임을 명시
    
- `@RequestMapping("/memo")`: 클래스 레벨에서 `/memo` 경로를 공통으로 설정
    
- `@Slf4j`: Lombok의 로깅 어노테이션 (`log.info()` 등 사용 가능)
    

---

### ✅ 주요 메서드 설명

#### 📍 `@InitBinder`

- 메서드명: `dataBinder`
    
- 역할: 커스텀 에디터 등록
    
- `registerCustomEditor(LocalDate.class, "dateTest", new DateTestEditor())`
    
    - `MemoDto`의 `dateTest` 필드에 대해 문자열 → LocalDate 변환 시 `DateTestEditor`를 사용
        
    - 예: `2024#04#24` → `2024-04-24`
        

#### 📍 `@GetMapping("/add")`

- 주소: `/memo/add` (GET)
    
- 역할: 등록 폼 페이지 요청 처리
    
- 리턴값 없음 → `memo/add.jsp`로 이동
    

#### 📍 `@PostMapping("/add")`

- 주소: `/memo/add` (POST)
    
- 파라미터: `@Valid MemoDto dto`, `BindingResult bindingResult`, `Model model`
    
- 유효성 검사 실패 시:
    
    - `bindingResult.hasErrors()` → true
        
    - 각 `FieldError`의 메시지를 로그로 출력하고 `Model`에 등록
        

---

### ✅ 커스텀 에디터 설명 - `DateTestEditor`

- 내부 클래스
    
- `PropertyEditorSupport` 상속
    
- `setAsText(String)` 오버라이딩
    
    - 빈 문자열이면 `LocalDate.now()` 사용
        
    - `#`을 `-`로 치환 후 `"yyyy-MM-dd"` 포맷으로 파싱
        
    - `setValue(LocalDate)` 호출로 DTO 필드에 값 저장
        

---

## 📌 요약

- `MemoController`는 메모 작성 폼의 유효성 검사를 수행
    
- `BindingResult`를 통해 DTO 검증 실패 시 에러 메시지를 필드별로 `Model`에 전달
    
- `@InitBinder`를 통해 `dateTest` 필드에 커스텀 날짜 변환기(`DateTestEditor`) 적용
    
- 사용자 입력값이 `2024#04#24`처럼 비표준일 경우도 자동 변환 가능하도록 처리함