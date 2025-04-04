# 📄 JSP - 로그인 상태 확인 및 로그인 폼 처리 예제

## 📌 개념 요약

로그인 폼에 접근할 때 이미 로그인된 사용자는 `session` 검사 후 메인 페이지로 리다이렉트하고, 로그인되지 않은 경우에만 폼을 보여주는 구조

---

## ✅ 주요 내용

- `session.getAttribute("isAuth") != null`  
    → 세션에 로그인 인증 속성이 있으면 로그인된 것으로 간주
    
- `response.sendRedirect("main.jsp")`  
    → 클라이언트를 `main.jsp`로 이동시킴 (새 요청 발생)
    
- `${param.message}`: 로그인 실패 등 메시지를 쿼리 파라미터로 전달받아 출력
    
- `${username_msg}`, `${password_msg}`: JSTL 표현식 사용, EL(Expression Language)로 메시지 출력
    
- `form` 태그: 로그인 정보를 `POST` 방식으로 서버에 전달
    

---

## 💻 코드 예시 (로그인 폼 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<% 
	// 로그인 상태 확인
	if(session.getAttribute("isAuth") != null){
		out.println("<script>alert('이미 로그인 상태입니다.')</script>");
		response.sendRedirect("./main.jsp"); // 메인 페이지로 이동
	}
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>로그인 폼</title>
<style>
	label { font-size: .8rem; }
	span { font-size: .5rem; color: red; }
</style>
</head>
<body>

<h1>LOGIN FORM</h1>

<!-- 쿼리 파라미터 메시지 출력 (ex. 로그인 실패 안내) -->
<div style="min-height:25px; font-size:.8rem; color:orange">
	${param.message}
</div>

<form action="${pageContext.request.contextPath}/C06/02/login.jsp" method="post">
	<div>
		<label>아이디 :</label><span>${username_msg}</span><br/>
		<input type="text" name="username"/>
	</div>
	<div>
		<label>패스워드 :</label><span>${password_msg}</span><br/>
		<input type="text" name="password"/>
	</div>
	<div>
		<button>로그인</button>
	</div>
</form>

</body>
</html>
```

---

## 📌 정리

- 로그인 상태 체크는 `session.getAttribute()`로 진행
    
- 로그인된 사용자는 폼 접근 차단 후 `main.jsp`로 이동
    
- 로그인 폼은 사용자 입력값 + 서버 전달 메시지를 함께 출력하도록 구성
    
- 로그인 실패 안내는 `${param.message}`로 처리
    

---

## 🔎 추가 개념

- `pageContext.request.contextPath`는 현재 웹 애플리케이션의 루트 경로 반환 → 프로젝트가 루트가 아닐 경우에도 경로 보정 가능
    
- 로그인 성공 시 `session.setAttribute("isAuth", true)` 등으로 인증 정보 저장
    
- JSTL이나 EL 표현식 사용 시, 해당 속성은 `request.setAttribute()`로 전달되어야 화면에 출력 가능
    

---