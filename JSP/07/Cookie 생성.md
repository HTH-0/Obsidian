# 📄 JSP - 쿠키 생성 및 저장 예제 (`Cookie` 객체 사용)

## 📌 개념 요약

`javax.servlet.http.Cookie` 객체를 사용해 브라우저에 쿠키를 생성하고, 응답으로 전송하여 저장할 수 있음. 쿠키는 **문자열 형태의 작은 데이터(최대 4KB)** 저장 용도로 사용됨.

---

## ✅ 주요 내용

- `new Cookie("이름", "값")`: 쿠키 생성 (문자열만 저장 가능)
    
- `cookie.setMaxAge(초)`: 쿠키 유지 시간 설정
    
    - `-1`: 브라우저 종료 시 삭제 (세션 쿠키)
        
    - `0`: 즉시 만료 (삭제용)
        
    - 양수: 지정한 초(seconds) 동안 유지
        
- `cookie.setPath("/")`: 적용 범위 지정 (전체 경로 적용)
    
- `response.addCookie(cookie)`: 클라이언트에게 쿠키 전달
    

---

## 💻 코드 예시 (createCookie.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!--  
	Cookie 저장 관련 정보:
	- 문자열만 저장 가능
	- 1개 크기 최대 4KB
	- 한 도메인당 최대 20개
	- 총 최대 300개까지 가능
	- 초과 시 오래된 쿠키부터 자동 삭제
-->

<%
	// 쿠키 1: 30초 유지
	Cookie cookie1 = new Cookie("myCookie1", "myCookie1Value");
	cookie1.setMaxAge(30); // 30초

	// 쿠키 2: 5분 유지
	Cookie cookie2 = new Cookie("myCookie2", "myCookie2Value");
	cookie2.setMaxAge(60 * 5); // 5분
	cookie2.setPath("/"); // 전체 애플리케이션 경로에 적용

	// 쿠키 브라우저로 전송
	response.addCookie(cookie1);
	response.addCookie(cookie2);
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키 생성 페이지</title>
</head>
<body>
	<a href="./getCookie.jsp">쿠키확인하기</a>
</body>
</html>
```

---

## 📌 정리

|설정 항목|설명|
|---|---|
|이름 & 값|문자열 형태로만 저장 가능|
|`setMaxAge()`|쿠키 유효 시간(초 단위)|
|`setPath()`|어떤 경로에서 쿠키를 사용할지 지정|
|`addCookie()`|쿠키를 응답에 추가해서 브라우저에 저장 지시|

---

## 🔎 추가 개념

- 한 도메인에서 `20개` 초과 저장 시 오래된 쿠키부터 제거됨
    
- 쿠키는 클라이언트에 저장되므로 **보안에 민감한 정보는 저장 금지**
    
- 쿠키는 매 요청마다 서버로 전송됨 → 너무 큰 데이터 저장은 지양
    

---