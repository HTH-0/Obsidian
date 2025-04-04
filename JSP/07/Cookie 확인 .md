# 📄 JSP - 쿠키 확인 페이지 예제 (`스크립틀릿 + 표현식` 사용)

## 📌 개념 요약

클라이언트에서 전달받은 쿠키 목록을 JSP에서 `request.getCookies()`로 받아 출력하며, 각 쿠키에 대해 삭제 링크도 함께 제공하는 구조

---

## ✅ 주요 내용

- `request.getCookies()`: 클라이언트에서 전달된 쿠키 배열 반환
    
- `for(Cookie cookie : cookies)`: 모든 쿠키 순회
    
- `<%= cookie.getName() %>`, `<%= cookie.getValue() %>`: 쿠키 이름과 값 출력
    
- 쿠키마다 삭제를 위한 `<a>` 태그로 링크 제공 (`deleteCookie.jsp?cookieName=...`)
    
- `System.out.println(...)`: 서버 콘솔(톰캣 로그)에 쿠키 정보 출력 (디버깅용)
    

---

## 💻 코드 예시 (getCookie.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>쿠키 확인 페이지</title>
</head>
<body>

<h1>쿠키 확인 (스크립틀릿 + 표현식)</h1>

<%
	Cookie[] cookies = request.getCookies();
	if(cookies != null){
		for(Cookie cookie : cookies){
			System.out.println("cookie : " + cookie.getName() + " : " + cookie.getValue());
%>
	<div>
		<a href="./deleteCookie.jsp?cookieName=<%= cookie.getName() %>">
			<%= cookie.getName() %> : <%= cookie.getValue() %>
		</a>
	</div>
<%
		}
	}
%>

<hr/>

<%-- 
<h1>쿠키 확인 (EL)</h1>
COOKIE1 값 : ${cookie.myCookie1.value}<br/>
COOKIE2 값 : ${cookie.myCookie2.value}<br/>
--%>

</body>
</html>
```

---

## 📌 정리

|항목|설명|
|---|---|
|`request.getCookies()`|클라이언트가 보낸 모든 쿠키 가져오기|
|`cookie.getName()`|쿠키 이름 반환|
|`cookie.getValue()`|쿠키 값 반환|
|EL 표현식 `${cookie.이름.value}`|쿠키 값을 EL로 출력 (단, 쿠키가 존재해야 함)|

---

## 🔎 추가 개념

- EL을 사용한 쿠키 접근은 `cookie.쿠키이름.value` 또는 `cookie["쿠키이름"].value`로 가능
    
- EL 표현식은 쿠키가 없을 경우 null을 출력함 (에러 발생하지 않음)
    
- 쿠키 삭제는 동일한 이름 + 경로로 `setMaxAge(0)` 설정한 쿠키를 클라이언트에 다시 보내야 유효
    

---