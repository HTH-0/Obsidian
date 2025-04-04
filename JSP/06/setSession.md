# 📄 JSP - request/session 속성 설정 및 전달 예제

## 📌 개념 요약

JSP에서 `request.setAttribute()`와 `session.setAttribute()`로 데이터를 저장한 뒤, 다른 페이지에서 해당 값을 출력할 수 있음

---

## ✅ 주요 내용

- `request.setAttribute("key", value)`: 현재 요청(Request) 객체에 데이터 저장 (forward로만 전달 가능)
    
- `session.setAttribute("key", value)`: 세션(Session) 객체에 데이터 저장 (브라우저 유지되는 동안 사용 가능)
    
- `request`는 요청마다 새로 생성되고, `session`은 클라이언트가 유지되는 동안 지속
    
- HTML의 `<a>` 태그를 사용하면 새로운 요청이 발생 → `request` 데이터는 전달되지 않음
    

---

## 💻 코드 예시 (속성 설정 JSP)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	// Request 속성 저장: 이 페이지 내에서만 유효
	request.setAttribute("ID1", "user1");
	request.setAttribute("PW1", "1111");

	// Session 속성 저장: 브라우저 세션 동안 유효
	session.setAttribute("ID2", "user2");
	session.setAttribute("PW2", "2222");
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>속성 저장 페이지</title>
</head>
<body>
	<!-- 다른 페이지로 이동 (새 요청 발생) -->
	<a href="./getSession.jsp">정보확인</a>
</body>
</html>
```

---

### 📂 연결된 페이지 (`getSession.jsp`) 예시

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>속성 출력</title>
</head>
<body>

	<h1>속성 확인</h1>

	<!-- request는 새 요청이라 null -->
	<div>REQUEST 속성 확인</div>
	ID : <%= request.getAttribute("ID1") %> <br />
	PW : <%= request.getAttribute("PW1") %> <br />

	<!-- session은 유지됨 -->
	<div>SESSION 속성 확인</div>
	ID : <%= session.getAttribute("ID2") %> <br />
	PW : <%= session.getAttribute("PW2") %> <br />

</body>
</html>
```

---

## 📌 정리

- `request`: 현재 요청에서만 유효 → `<a href>`나 `redirect` 사용 시 사라짐
    
- `session`: 페이지 이동, 새 요청에도 유지됨 → 로그인 정보 저장 등에 유용
    
- `request` 값을 다른 페이지로 넘기려면 `forward` 사용 필요 (`RequestDispatcher` 활용)
    

---

## 🔎 추가 개념

- `session.invalidate()`로 세션 강제 종료 가능
    
- 요청 유지가 필요한 경우 `<jsp:forward>` 또는 `RequestDispatcher.forward()` 사용
    
- `response.sendRedirect()`는 새로운 요청을 발생시키므로 `request` 데이터 소실됨
    

---