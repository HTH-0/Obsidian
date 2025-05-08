---
aliases:
  - form 태그/ global error
---

---

# 📋 Spring Form 태그 + 전역 에러 출력 방식 정리

## ✅ 1. spring-form 태그 선언 (JSP 상단)

```jsp
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```

- Spring에서 제공하는 `form:form`, `form:input`, `form:errors` 등을 사용하기 위한 선언
    

---

## ✅ 2. form:form 방식의 바인딩 예시 (JSP)

```jsp
<form:form method="post" modelAttribute="memoDto" action="/memo/add">

    <div>
        <label>제목</label>
        <form:input path="title"/>
        <form:errors path="title" cssClass="error"/>
    </div>

    <div>
        <label>내용</label>
        <form:input path="content"/>
        <form:errors path="content" cssClass="error"/>
    </div>

    <div>
        <label>날짜</label>
        <form:input path="dateTest"/>
        <form:errors path="dateTest" cssClass="error"/>
    </div>

    <button type="submit">등록</button>
</form:form>
```

- `modelAttribute="memoDto"`: 컨트롤러에서 전달한 DTO 객체 이름과 일치해야 함
    
- `form:input`의 `path`는 DTO의 필드명과 매핑됨
    
- `form:errors`는 해당 필드에 대한 유효성 실패 메시지를 출력
    

---

## ✅ 3. 컨트롤러 예시

```java
@GetMapping("/add")
public String addForm(Model model) {
    model.addAttribute("memoDto", new MemoDto());
    return "memo/add";
}

@PostMapping("/add")
public String addSubmit(@Valid @ModelAttribute("memoDto") MemoDto dto,
                        BindingResult result) {
    if (result.hasErrors()) {
        return "memo/add";
    }
    // 저장 로직
    return "redirect:/memo/success";
}
```

---

## ✅ 4. 전체 에러 출력 예시 (전역 오류 메시지)

```jsp
<form:errors path="*" cssClass="global-error"/>
```

- `path="*"`는 모든 필드에 대해 발생한 오류를 한 번에 출력함
    
- 페이지 상단에 전체 오류 메시지를 표시하고 싶을 때 사용
    

---

## 📌 요약

|항목|설명|
|---|---|
|form:form|DTO를 기준으로 바인딩할 때 사용|
|form:input|path로 DTO 필드와 연결|
|form:errors|해당 필드의 유효성 메시지 출력|
|path="*"|전체 에러 출력용|
|modelAttribute|form 바인딩 대상 DTO 이름 지정|

---
