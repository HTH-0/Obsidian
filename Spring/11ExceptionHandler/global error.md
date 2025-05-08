
---

## 🧾 global_error.jsp

### 📦 전체 코드

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>예외 발생</h1>
	<h3>${ex}</h3>
</body>
</html>
```

---

## 🔍 코드 분석

- `<%@ page ... %>`  
    → JSP 페이지 설정. UTF-8로 인코딩되어 한글 및 특수문자 지원.
    
- `<h1>예외 발생</h1>`  
    → 에러 페이지의 제목 표시
    
- `${ex}`  
    → `GlobalExceptionHandler`에서 `model.addAttribute("ex", e)`로 넘긴 예외 객체 출력  
    → 예외 클래스명과 메시지 (`java.io.FileNotFoundException: ...`) 형태로 나타남
    

---

## 📌 요약

- 이 JSP는 `@ControllerAdvice`를 통한 전역 예외 처리 시 사용자에게 에러 메시지를 보여주는 페이지
    
- `${ex}`를 통해 예외 객체의 toString 결과를 출력하므로, 간단한 디버깅용 또는 사용자 안내용으로 사용 가능
    
- 필요 시 `${ex.message}` 또는 `<c:forEach>` 등을 사용해 스택트레이스나 상세 내용도 확장 가능
    

---
