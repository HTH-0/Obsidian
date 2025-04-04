# 📄 JSP - 쿠키 Path 설정 예제 (`setPath()` 활용)

## 📌 개념 요약

쿠키는 `setPath()` 설정에 따라 **어떤 요청에 포함되어 서버로 전송될지**가 결정됨. 같은 도메인이라도 **경로(Path)** 설정에 따라 쿠키 전달 여부가 달라짐.

---

## ✅ 주요 내용

- `Cookie(name, value)`: 쿠키 객체 생성
    
- `cookie.setMaxAge(초)`: 쿠키 유효 시간 설정
    
- `cookie.setPath()`:
    
    - `"/"` → 모든 경로에 전달 (전역 쿠키)
        
    - `"./"` → 현재 경로(폴더) 하위만 전달
        
    - `"/경로/파일.jsp"` → 해당 JSP 요청에만 쿠키 전달
        

---

## 💻 코드 예시 (createCookiePath.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	// 전역 경로 쿠키: 모든 페이지에 전송됨
	Cookie cookie1 = new Cookie("c1", "v1");
	cookie1.setMaxAge(60 * 10); // 10분
	cookie1.setPath("/");

	// 상대 경로 쿠키: 현재 디렉토리 기준(주의: 실제 사용은 권장되지 않음)
	Cookie cookie2 = new Cookie("c2", "v2");
	cookie2.setMaxAge(60 * 10);
	cookie2.setPath("./");

	// 특정 JSP 경로에만 전송되는 쿠키
	Cookie cookie3 = new Cookie("c2", "v2");
	cookie3.setMaxAge(60 * 10);
	cookie3.setPath("/01JSP/C07/02/getCookie.jsp");

	// 쿠키 브라우저로 전송
	response.addCookie(cookie1);
	response.addCookie(cookie2);
	response.addCookie(cookie3);
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키 Path 설정 예제</title>
</head>
<body>
	<a href="./getCookie.jsp">쿠키 확인하기</a>
</body>
</html>
```

---

## 📌 정리

|설정값|설명|
|---|---|
|`/`|사이트 내 **모든 요청**에 쿠키 전송됨|
|`./`|현재 JSP가 있는 폴더 이하 경로에만 적용 (비권장)|
|`/경로/파일.jsp`|특정한 경로/파일로 요청될 때만 쿠키 포함|

---

## 🔎 추가 개념

- **`Path`가 다르면 같은 이름의 쿠키도 서로 다른 것으로 처리됨**
    
- `setPath()`는 브라우저가 요청을 보낼 때 해당 요청 URL이 이 경로로 **시작할 경우에만 쿠키를 전송**
    
- 일반적으로는 `/` 설정이 가장 많이 사용됨 (전역 사용 가능)
    

---