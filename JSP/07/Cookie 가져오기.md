# 📄 JSP - 쿠키 확인 및 삭제 링크 생성 예제

## 📌 개념 요약

`request.getCookies()`를 통해 클라이언트에서 전달된 모든 쿠키를 가져오고, 이를 출력하거나 삭제할 수 있도록 동적 링크를 생성하는 예제

---

## ✅ 주요 내용

- `request.getCookies()`는 클라이언트가 보낸 모든 쿠키 배열을 반환
    
- 쿠키가 존재하면 반복문(`for`)으로 각각 이름(name)과 값(value) 출력
    
- 각 쿠키마다 **삭제 요청 링크** (`deleteCookie.jsp`) 생성
    
- `${cookie.쿠키이름.value}` 표현은 EL을 사용할 때 쿠키 값 출력 가능 (주석 처리되어 있음)
    

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
		<!-- 쿠키 삭제 링크 -->
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
<h1>쿠키 확인 (EL 사용)</h1>
COOKIE1의 값 : ${cookie.myCookie1.value}<br/>
COOKIE2의 값 : ${cookie.myCookie2.value}<br/>
--%>

</body>
</html>
```

---

## 📌 정리

- `request.getCookies()`로 모든 쿠키 확인 가능
    
- `cookie.getName()` / `getValue()`로 개별 속성 접근
    
- 쿠키 삭제는 `<a href="deleteCookie.jsp?cookieName=...">` 방식으로
    
- 주석 처리된 `${cookie.쿠키이름.value}`는 EL 방식 쿠키 접근 (단, 존재하지 않으면 null)
    

---

## 🔎 추가 개념

- EL로 쿠키 접근 시 `cookie["myCookie1"].value` 형식도 사용 가능
    
- 쿠키 삭제 시에는 이름과 path 일치, `setMaxAge(0)` 필수
    
- 브라우저는 같은 이름의 쿠키가 여러 개일 수 없고, 가장 마지막으로 설정된 것이 적용됨
    

---