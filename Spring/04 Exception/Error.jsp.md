# 🧾 에러 처리 JSP 페이지 (`except/error.jsp` 또는 `global_error.jsp` 용)

## 📦 전체 코드

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true" %>
    <!-- isErrorPage 추가함 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>EXCEPT / ERROR</h1>
	
	EXCEPTION : <%=exception%> <br/>
	
</body>
</html>
```

---

## 🔍 코드 분석

### ✅ `isErrorPage="true"`

- 이 속성을 JSP 페이지 상단에 선언해야 **`exception` 내장 객체** 사용 가능
    
- 예외 처리용 페이지임을 명시하며, `ExceptionHandler` 또는 `web.xml`의 에러 페이지로 지정된 경우에 사용됨
    

### ✅ `<%=exception%>`

- JSP 내장 객체 `exception`을 사용해 예외 메시지를 출력
    
- 이는 `Throwable` 객체이며, 예외 종류와 메시지를 포함함
    
- `isErrorPage="true"`가 없으면 컴파일 오류 발생
    

---

## 📌 요약

- 예외 발생 시 사용자가 볼 에러 처리 페이지
    
- `isErrorPage="true"`로 설정하여 `exception` 객체 사용 가능하게 함
    
- 전역 예외 처리기(`GlobalExceptionHandler`)나 개별 컨트롤러의 `@ExceptionHandler`에서 이 JSP로 포워딩할 수 있음
    
- 실제 서비스에서는 사용자 친화적인 메시지나 오류 코드 등을 포함하여 UI 개선이 필요함 (e.g., `exception.getMessage()` 또는 로그 기록 등 활용)