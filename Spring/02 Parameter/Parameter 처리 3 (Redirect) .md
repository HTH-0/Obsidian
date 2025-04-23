좋아, 이 코드는 **Spring MVC의 리다이렉트 흐름과 `RedirectAttributes`를 활용한 파라미터 전달**을 실습하는 예제야. 구조도 깔끔하고 흐름도 정확해. 아래처럼 옵시디언에 정리하기 좋게 문서화해줄게.

---

# 🔀 리다이렉트와 파라미터 전달 실습

## 📦 전체 코드

```java
// Redirect 시작
@GetMapping("/redirect1")
public String r1(Model model, RedirectAttributes redirectAttributes) {
	log.info("/param/redirect1...");
	redirectAttributes.addAttribute("r1", "r1Value"); // 쿼리스트링으로 전달
	return "redirect:/param/redirect2";
}

// 중간 처리 및 파라미터 전달 확장
@GetMapping("/redirect2")
public String r2(
	Model model,
	@RequestParam("r1") String r1,
	RedirectAttributes redirectAttributes
) {
	log.info("/param/redirect2... r1 : " + r1);
	redirectAttributes.addAttribute("r1", r1);         // 다시 전달
	redirectAttributes.addAttribute("r2", "r2Value");   // 새 파라미터 추가
	return "redirect:/param/redirect_result";
}

// 최종 도착지
@GetMapping("/redirect_result")
public void r_result(
	Model model,
	@RequestParam("r1") String r1,
	@RequestParam("r2") String r2
) {
	model.addAttribute("r1", r1);
	model.addAttribute("r2", r2);
	model.addAttribute("r3", "r3Value"); // 고정값 추가
	log.info("/param/redirect_result..."); 
}
```

---

## 🔍 코드 분석

### ✅ `/redirect1`

- `RedirectAttributes.addAttribute()`를 통해 `r1=r1Value` 전달
    
- `Model.addAttribute()`는 리다이렉트 시 적용 안 됨 (새 요청이기 때문)
    

### ✅ `/redirect2`

- `@RequestParam("r1")`으로 전달된 값을 수신
    
- `r1`, `r2` 파라미터를 다음 리다이렉트 요청에 추가
    

### ✅ `/redirect_result`

- URL의 `r1`, `r2`를 `@RequestParam`으로 받음
    
- 최종적으로 `Model`에 `r1`, `r2`, `r3`를 담아 JSP에서 출력 가능
    

---

## ✅ 리다이렉트 동작 방식

|메서드|리턴값|결과 URI|
|---|---|---|
|`redirect1()`|`redirect:/param/redirect2`|`/param/redirect2?r1=r1Value`|
|`redirect2()`|`redirect:/param/redirect_result`|`/param/redirect_result?r1=r1Value&r2=r2Value`|
|`redirect_result()`|void → 뷰 이름 생략|`/WEB-INF/views/param/redirect_result.jsp` 로 포워딩됨|

---

## 🔎 리다이렉트에서 데이터 전달 방법

|전달 방식|설명|비고|
|---|---|---|
|`RedirectAttributes.addAttribute()`|쿼리 파라미터(`?key=value`) 형식으로 전달|리다이렉트에서 유일하게 사용 가능|
|`Model.addAttribute()`|내부 포워딩에서만 사용|리다이렉트 시에는 소용 없음|

---

## 📌 요약

- 리다이렉트는 **새 요청**이므로 기존의 `Model`은 유지되지 않음
    
- 값을 넘기려면 반드시 `RedirectAttributes.addAttribute()` 사용
    
- 최종적으로 전달된 파라미터는 `@RequestParam`으로 받아야 함
    
- JSP에서는 `${r1}`, `${r2}`, `${r3}`로 출력 가능
    

필요하면 `redirect_result.jsp` 예시도 만들어줄 수 있어. 요청해줘!