
# 📄 JSP View 파일 정리 2

---

## ❌ error.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<h1>USER ERROR PAGE</h1>
	status : ${ status }		<br/>
	message : ${ message }		<br/>
	exception : ${ exception }	<br/>
</body>
</html>
```

### 🧠 설명

- 예외 발생 시 전용 에러 화면으로 포워딩됨
    
- `request.setAttribute()`를 통해 전달된 오류 정보 출력
    
    - `status` → 성공/실패 여부 (`false`)
        
    - `message` → 예외 메시지
        
    - `exception` → 예외 객체 (`e`)
        

---

## 🔓 logout.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
	<%@include file="/resources/layouts/link.jsp" %>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<div class="wrapper">
		<header>
			<%@include file="/resources/layouts/topHeader.jsp" %>
			<%@include file="/resources/layouts/nav.jsp" %>
		</header>
		<main class="layout">
			<h1>USER LOGOUT</h1>
			<form action="${pageContext.request.contextPath}/user/logout" method="post">
				<button type="submit">로그아웃</button>
			</form>
			<div>
				${username_err}
			</div>
		</main>
		<%@include file="/resources/layouts/footer.jsp" %>
	</div>
</body>
</html>
```

### 🧠 설명

- 로그아웃 버튼을 누르면 `/user/logout`으로 POST 전송됨
    
- 실제 로그아웃 처리 컨트롤러는 `UserLogoutController`
    
- 필요에 따라 메시지 또는 에러 표시 영역도 존재함 (`username_err`)
    

---

## 🧑‍💼 manager.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
	<%@include file="/resources/layouts/link.jsp" %>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<div class="wrapper">
		<header>
			<%@include file="/resources/layouts/topHeader.jsp" %>
			<%@include file="/resources/layouts/nav.jsp" %>
		</header>
		<main class="layout">
			<h1>MANAGER</h1>
		</main>
		<%@include file="/resources/layouts/footer.jsp" %>
	</div>
</body>
</html>
```

### 🧠 설명

- 매니저 전용 메인 화면
    
- 권한이 `ROLE_MANAGER`인 사용자만 접근 가능 (컨트롤러 또는 필터에서 제어 예정)
    
- 템플릿 요소와 기본 구조 동일함
    

---

## 👤 user.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
	<%@include file="/resources/layouts/link.jsp" %>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<div class="wrapper">
		<header>
			<%@include file="/resources/layouts/topHeader.jsp" %>
			<%@include file="/resources/layouts/nav.jsp" %>
		</header>
		<main class="layout">
			<h1>USER</h1>
		</main>
		<%@include file="/resources/layouts/footer.jsp" %>
	</div>
</body>
</html>
```

### 🧠 설명

- 일반 사용자 전용 메인 페이지
    
- 로그인 후 `ROLE_USER` 권한일 경우 이 뷰로 연결됨
    

---

## ✅ 공통 요소 정리

- 모든 JSP는 `wrapper` div로 감싸진 일관된 레이아웃 사용
    
- `topHeader.jsp`, `nav.jsp`, `footer.jsp`는 공통 레이아웃 파일
    
- 뷰 파일은 모두 `WEB-INF` 내부에 위치한다고 가정 → 직접 접근 불가, 반드시 컨트롤러 통해 접근
    

---
