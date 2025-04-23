# 📥 다양한 파라미터 처리 방식 실습 컨트롤러

## 📦 전체 코드

```java
@Controller
@Slf4j
@RequestMapping("/param")
public class ParameterController {
	
	@RequestMapping(value="/p01", method=RequestMethod.GET)
	public void p01(@RequestParam(value="name", required=false) String name) {
		log.info("GET /param/p01..." + name);
	}
	
	@GetMapping("/p02")
	public void p02(@RequestParam(value="name", required=true) String name) {
		log.info("GET /param/p02..." + name);
	}
	
	@PostMapping(value="/p03")
	public void p03(@RequestParam(value="name", required=true) String name) {
		log.info("POST /param/p03..." + name);
	}
	
	@PostMapping(value="/p04")
	public void p04(@RequestBody String name) {
		log.info("POST /param/p04..." + name);
	}
	
	@RequestMapping(value="/p05", method=RequestMethod.GET)
	public void p05(@RequestParam(value="name", defaultValue="홍길동") String name) {
		log.info("GET /param/p05..." + name);
	}
	
	@RequestMapping(value="/p06", method=RequestMethod.GET)
	public void p06(
			@RequestParam(value="name")	String name,
			@RequestParam(value="age") int age,
			@RequestParam(value="addr") String addr
	) {
		log.info("GET /param/p06..." + name + " " + age + " " + addr);
	}
	
	@RequestMapping(value="/p07", method=RequestMethod.GET)
	public void p07(@ModelAttribute PersonDto dto) {
		log.info("GET /param/p07..." + dto);
	}
	
	@RequestMapping(value="/p08/{username}/{age}/{addr}", method=RequestMethod.GET)
	public void p08(
			@PathVariable("username") String username,
			@PathVariable int age,
			@PathVariable String addr
	) {
		log.info("GET /param/p08..." + username + " " + age + " " + addr);
	}
	
	@RequestMapping(value="/p09/{username}/{age}/{addr}", method=RequestMethod.GET)
	public void p09(@ModelAttribute PersonDto dto) {
		log.info("GET /param/p09..." + dto);
	}
	
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Controller`: Spring MVC 컨트롤러 역할
    
- `@Slf4j`: `log.info()` 등 로그 출력 지원
    
- `@RequestMapping("/param")`: 컨트롤러 공통 URI prefix 설정
    

---

### ✅ 주요 메서드 설명

- `p01()`
    
    - `@RequestParam(required=false)`로 파라미터 생략 가능
        
    - 예: `/param/p01`, `/param/p01?name=kim`
        
- `p02()`
    
    - `@RequestParam(required=true)` → 필수 파라미터 없으면 400 에러
        
- `p03()`
    
    - `POST` 요청 + `@RequestParam` 방식
        
- `p04()`
    
    - `@RequestBody String name` → JSON 혹은 raw 텍스트 요청 바디 처리
        
- `p05()`
    
    - `@RequestParam(defaultValue="홍길동")` → 기본값 설정
        
- `p06()`
    
    - 여러 `@RequestParam`을 통해 다중 파라미터 수신
        
    - 예: `/param/p06?name=kim&age=20&addr=seoul`
        
- `p07()`
    
    - `@ModelAttribute PersonDto` → 요청 파라미터를 DTO에 바인딩 (name, age, addr 필드 필요)
        
- `p08()`
    
    - `@PathVariable`로 URI 경로 값을 직접 추출
        
    - 예: `/param/p08/kim/22/seoul`
        
- `p09()`
    
    - URI 경로 값을 `@ModelAttribute` DTO에 자동 매핑
        
    - 예: `/param/p09/kim/22/seoul` → `PersonDto`에 자동 바인딩
                

---

## 📌 요약

|처리 방식|어노테이션|설명|
|---|---|---|
|쿼리 파라미터|`@RequestParam`|동기, 기본 방식|
|바디(raw/json)|`@RequestBody`|비동기, JSON 등 처리|
|DTO 매핑|`@ModelAttribute`|파라미터 → 객체 자동 매핑|
|경로 변수|`@PathVariable`|RESTful 경로값 추출|

- `@ModelAttribute`는 query param 또는 path variable과 모두 호환됨
    
- `void` 리턴은 경로와 동일한 뷰로 포워딩 됨 (예: `/param/page01` → `page01.jsp`)
    
