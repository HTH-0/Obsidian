
---

# 📄 다양한 방식의 요청 파라미터 수신 및 뷰 이동 실습

## 📦 전체 코드

```java
@GetMapping("/page01")
public void page01(PersonDto dto, Model model) {
	log.info("GET /param/page01..." + dto);
	model.addAttribute("dto", dto);
	model.addAttribute("test", "test1Value");
}

@GetMapping("/page02")
public String page02(PersonDto dto, Model model) {
	log.info("GET /param/page02..." + dto);
	model.addAttribute("dto", dto);
	model.addAttribute("test", "test2Value");
	return "param/page01";
}

@GetMapping("/page03/{username}/{age}/{addr}")
public String page03(PersonDto dto, Model model) {
	log.info("GET /param/page03..." + dto);
	model.addAttribute("dto",dto);
	model.addAttribute("test","test3Value");
	return "param/page01";
}

@GetMapping("/page04/{username}/{age}/{addr}")
public ModelAndView page04(PersonDto dto) {
	log.info("GET /param/page04..." + dto);
	ModelAndView modelAndView = new ModelAndView();
	modelAndView.addObject("dto", dto);
	modelAndView.setViewName("param/page01");
	return modelAndView;
}

@GetMapping("/page05")
public String page05(HttpServletRequest req, HttpServletResponse resp) {
	log.info("GET /param/page05...");
	String name = req.getParameter("username");
	int age = Integer.parseInt(req.getParameter("age"));
	String addr = req.getParameter("addr");
	log.info(name + " " + age);
	PersonDto dto = new PersonDto(name, age, addr);
	req.setAttribute("dto", dto);
	HttpSession session = req.getSession();
	log.info("session : " + session);
	return "param/page01";
}
```

---

## 🔍 코드 분석

### ✅ `page01` - 기본 DTO 바인딩 + `Model` 전달

- `PersonDto`로 자동 바인딩 (쿼리 파라미터 사용)
    
- 반환형 `void` → `/WEB-INF/views/param/page01.jsp` 자동 매핑
    
- `model.addAttribute()`로 DTO와 테스트값 전달
    

---

### ✅ `page02` - 뷰 경로 수동 지정 (`return String`)

- 요청 처리 후 `param/page01.jsp`로 이동 (뷰 이름 직접 반환)
    
- `model`을 통해 데이터 전달
    

---

### ✅ `page03` - PathVariable + DTO 바인딩

- URL 경로 → DTO 바인딩 예시
    
- `GET /param/page03/kim/20/seoul` → `dto.username = "kim"`, `dto.age = 20`, `dto.addr = "seoul"`
    
- 뷰 이동은 `"param/page01"`
    

---

### ✅ `page04` - `ModelAndView` 방식

- DTO를 `ModelAndView.addObject()`로 전달
    
- 뷰 이름을 `setViewName()`으로 지정
    
- 반환형을 `ModelAndView`로 사용할 경우 명시적 표현 가능
    

---

### ✅ `page05` - 전통 Servlet 방식 (HttpServletRequest)

- `req.getParameter()`로 수동 파라미터 추출
    
- DTO 생성 후 `req.setAttribute()`로 request scope에 저장
    
- `session` 객체도 사용 가능 (예: 로그인 시 활용)
    

---

## 📌 요약

|메서드|주요 특징|데이터 전달 방식|뷰 이동 방식|
|---|---|---|---|
|`page01`|DTO 바인딩, void 반환|`Model`|자동 경로 매핑|
|`page02`|DTO 바인딩, 명시적 뷰|`Model`|return "param/page01"|
|`page03`|PathVariable + DTO|`Model`|return "param/page01"|
|`page04`|ModelAndView 사용|`ModelAndView`|return 객체로 이동|
|`page05`|Servlet 방식 수동 처리|`HttpServletRequest.setAttribute()`|return "param/page01"|

---

