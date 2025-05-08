
---

# ✅ Spring MVC 유효성 검사 및 바인딩 정리

## 📦 주요 코드 예제

### 1. `@Valid` + `BindingResult` 조합

```java
@PostMapping("/add")
public void add_post(@Valid MemoDto dto, BindingResult bindingResult, Model model) throws Exception {
    log.info("POST /memo/add." + dto);

    if (bindingResult.hasErrors()) {
        for (FieldError error : bindingResult.getFieldErrors()) {
            log.info("Error Field: " + error.getField() + " / Msg: " + error.getDefaultMessage());
            model.addAttribute(error.getField(), error.getDefaultMessage());
        }
        return;
    }

    boolean isAdded = memoServiceImpl.registrationMemo(dto);
}
```

- `@Valid`를 통해 DTO 필드에 선언된 유효성 어노테이션(@NotNull 등)을 검사
    
- `BindingResult`는 검증 결과를 담는 객체로 반드시 `@Valid` **뒤에 위치**
    

---

### 2. `@InitBinder`와 커스텀 바인딩 (LocalDate)

```java
//@InitBinder
//public void dataBinder(WebDataBinder webDataBinder) {
//    log.info("MemoController's dataBinder..." + webDataBinder);
//    webDataBinder.registerCustomEditor(LocalDate.class, "dateTest", new DateTestEditor());
//}
```

- 주석 처리되어 있으나, 날짜 형식을 커스터마이징할 때 유용
    

---

### 3. 커스텀 에디터 클래스: `DateTestEditor`

```java
private static class DateTestEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String dateTest) throws IllegalArgumentException {
        log.info("DateTestEditor's setAsText..." + dateTest);
        LocalDate date = null;

        if (dateTest.isEmpty()) {
            date = LocalDate.now();
        } else {
            // yyyy#MM#dd → yyyy-MM-dd
            dateTest = dateTest.replaceAll("#", "-");
            date = LocalDate.parse(dateTest, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
        }

        setValue(date);
    }
}
```

- `PropertyEditorSupport`를 상속하여 커스텀 문자열 → 객체 변환 로직 구현
    

---

## 🔍 핵심 개념 정리

|항목|설명|
|---|---|
|`@Valid`|DTO 객체의 필드에 대한 유효성 검사 수행|
|`BindingResult`|검사 결과 저장. 위치 중요 (`@Valid` 바로 뒤)|
|`FieldError`|유효성 실패한 필드와 메시지를 순회 처리|
|`@InitBinder`|커스텀 바인딩 적용 시 사용 (문자열 → 객체 변환)|
|`PropertyEditorSupport`|기본 형식 외의 데이터 포맷 처리 시 사용|

---

## 📌 요약

- 유효성 검사는 `@Valid`와 `BindingResult`를 함께 사용하며, 순서가 매우 중요
    
- 바인딩 에러 발생 시 `bindingResult.hasErrors()`로 체크
    
- 커스텀 타입(LocalDate 등)은 `@InitBinder + CustomEditor` 조합으로 처리
    
- 에러 메시지는 JSP에 개별 필드별로 출력 가능 (`${id}`, `${title}` 등)
    

---
