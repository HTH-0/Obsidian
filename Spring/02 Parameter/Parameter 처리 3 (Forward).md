
---

# 🔁 Forward 체인 처리 구조

## 📦 전체 코드

```java
@GetMapping("/forward1")
public String f1(Model model) {
	log.info("param/forward1...");
	model.addAttribute("f1", "f1Value");
	return "forward:/param/forward2";
}

@GetMapping("/forward2")
public String f2(Model model) {
	log.info("param/forward2...");
	model.addAttribute("f2","f2Value");
	return "forward:/param/forward3";
}

@GetMapping("/forward3")
public String f3(Model model) {
	log.info("param/forward3...");
	model.addAttribute("f3","f3Value");
	return "/param/forward_result";  // ❌ 여기서 문제가 발생
}
```

---

## 🔍 문제 포인트

```java
return "/param/forward_result";
```

- 이렇게 작성하면 ViewResolver가 `forward:/WEB-INF/views/param/forward_result.jsp`로 해석하지 않음
    
- 슬래시(`/`)로 시작하는 뷰 이름은 **정상 뷰 경로로 인식되지 않음**
    
- 결과적으로 404 또는 경로 오류 발생 가능성 있음
    

---

## ✅ 올바른 수정

### 방법 1: JSP 뷰로 이동 (ViewResolver 사용)

```java
return "param/forward_result";  // ✅ 뷰 이름만 반환
```

- ViewResolver에 의해 `/WEB-INF/views/param/forward_result.jsp`로 포워딩됨
    

---

### 방법 2: Controller method가 있을 경우 (지금은 해당 안 됨)

```java
return "forward:/param/forward_result";  // 만약 해당 URI를 처리하는 컨트롤러가 있다면 가능
```

---

## ✅ 요약

|목적|리턴 방식|설명|
|---|---|---|
|JSP 뷰 이동|`"param/forward_result"`|ViewResolver가 처리|
|컨트롤러 메서드로 이동|`"forward:/param/..."`|DispatcherServlet이 해당 메서드 실행|

---
