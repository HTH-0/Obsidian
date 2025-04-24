# 👤 UserController - 회원가입 커스텀 바인딩 처리

## 📦 전체 코드

```java
@Controller
@Slf4j
public class UserController {

	@InitBinder
	public void dataBinder(WebDataBinder webDataBinder) {
		log.info("UserController's DataBinder ..." + webDataBinder);
		webDataBinder.registerCustomEditor(LocalDate.class, "birthday", new DateTestEditor());
		webDataBinder.registerCustomEditor(String.class, "phone", new PhoneTestEditor());
	}

	@GetMapping("/join")
	public void join() {
		log.info("GET /join ...");
	}

	@PostMapping("/join")
	public void join_post(@Valid UserDto userDto, BindingResult bindingResult, Model model) {
		log.info("POST /join..." + userDto);
		
		if(bindingResult.hasErrors()) {
			for(FieldError error : bindingResult.getFieldErrors()) {			
				log.info("Error field : " + error.getField() + "Error Msg : " + error.getDefaultMessage());
				model.addAttribute(error.getField(), error.getDefaultMessage());
			}
		}
	}

	private static class DateTestEditor extends PropertyEditorSupport {
		@Override
		public void setAsText(String birthday) throws IllegalArgumentException {
			log.info("DataTestEditor's setAsText invoke..." + birthday);
			LocalDate date = null;
			if(birthday.isEmpty()) {
				date = LocalDate.now();
			} else {
				birthday = birthday.replaceAll("~","-");
				date = LocalDate.parse(birthday, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
			}
			setValue(date);
		}
	}

	private static class PhoneTestEditor extends PropertyEditorSupport {
		@Override
		public void setAsText(String text) throws IllegalArgumentException {
			log.info("PhoneEditor setAsText invoke... " + text);
			if (text == null || text.trim().isEmpty()) {
				setValue("01000000000");
			} else {
				setValue(text.replaceAll("-", ""));
			}
		}
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Controller`: 해당 클래스가 Spring MVC 컨트롤러임을 선언
    
- `@Slf4j`: Lombok에서 로그 객체(log) 자동 생성
    
- `@InitBinder`: 커스텀 변환기(`PropertyEditor`)를 등록할 때 사용
    

---

### ✅ 주요 메서드 설명

#### 📍 `@InitBinder` - 커스텀 에디터 등록

- `LocalDate.class`, `String.class`에 대한 커스텀 에디터를 각각 등록
    
    - `"birthday"` 필드 → `DateTestEditor`
        
    - `"phone"` 필드 → `PhoneTestEditor`
        

#### 📍 `@GetMapping("/join")`

- 회원가입 폼 뷰 호출
    
- 뷰 이름은 `/join.jsp`로 매핑됨 (void 반환)
    

#### 📍 `@PostMapping("/join")`

- 회원가입 제출 처리
    
- `@Valid`로 유효성 검사 수행
    
- `BindingResult`를 통해 유효성 검사 결과 확인
    
- 에러가 있으면 `Model`에 필드별 메시지를 추가 (`error.getField()`를 key로 사용)
    

---

### ✅ 커스텀 에디터 설명

#### 🔧 `DateTestEditor`

- 생일 입력 문자열을 `LocalDate`로 변환
    
- 포맷: `yyyy~MM~dd` → `yyyy-MM-dd`로 변환 후 파싱
    
- 빈 값일 경우 현재 날짜로 설정
    

#### 🔧 `PhoneTestEditor`

- 전화번호 문자열을 하이픈(-) 제거 후 저장
    
- 빈 값 입력 시 기본값 `"01000000000"`으로 설정
    

---

## 📌 요약

- `UserController`는 회원가입 폼을 처리하며, `birthday`와 `phone` 필드를 커스텀 포맷으로 변환함
    
- `@InitBinder`를 활용해 입력값의 전처리를 `PropertyEditorSupport`로 처리
    
- 유효성 검사 결과는 `BindingResult`를 통해 확인하고 `Model`에 에러 메시지를 전달함
    
- 이 구조는 폼 입력값을 유연하게 처리하고 사용자 입력 실수를 보완하는 데 유용함