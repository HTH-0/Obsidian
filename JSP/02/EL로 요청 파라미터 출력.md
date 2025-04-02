# 📄 EL(Expression Language)로 요청 파라미터 출력 (JSP + EL)

## 📌 개념 요약

JSP에서 `EL(Expression Language)`를 사용하면 자바 코드 없이도 요청 파라미터를 간단하게 출력할 수 있음

---

## ✅ 주요 내용

- `request.getParameter()`를 대체하여 `${param.파라미터명}` 형태로 데이터 출력
    
- `EL`은 JSP 2.0 이상에서 지원되는 표현 언어
    
- `request.setCharacterEncoding("UTF-8")`은 POST 방식일 때 반드시 필요
    
- `response.setContentType("text/html; charset=UTF-8")`은 한글 깨짐 방지
    
- HTML 요소에서도 EL을 통해 동적 스타일 적용 가능 (ex. `background-color`)
    

---

## 💻 코드 예시 (C02Request_Process.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%
	// POST 방식 문자 인코딩 처리
	request.setCharacterEncoding("UTF-8");
	response.setContentType("text/html; charset=UTF-8");
%>

<!-- EL을 활용한 요청 파라미터 출력 -->
<!doctype html>
<html lang="ko">
<head>
	<meta charset="UTF-8" />
	<title>Document</title>
</head>
<body style="background-color:${param.bgcolor}">

	EL_USERNAME : ${param.username} <br/>
	EL_PASSWORD : ${param.password} <br/>

</body>
</html>
```

---

## 📌 정리

- `${param.파라미터명}`은 request.getParameter()와 동일한 기능
    
- Java 코드를 최소화하면서도 표현식으로 데이터를 출력할 수 있어 유지보수가 쉬움
    
- JSTL과 함께 사용할 때 매우 유용함
    

---

## 🔎 추가 개념

- **EL 기본 객체**
    
    - `${param.xxx}` → request.getParameter("xxx")
        
    - `${header.xxx}` → request.getHeader("xxx")
        
    - `${cookie.xxx}` → 쿠키 접근
        
    - `${sessionScope.xxx}` → 세션 접근 등
        
- **EL vs Scriptlet**
    
    - EL은 표현만 가능하고 로직 처리 불가능
        
    - 복잡한 분기나 반복은 JSTL 또는 자바 코드로 처리해야 함
        

---