
# 🌐 JSP View 파일 정리 - 10번 프로젝트

---

## 🧑‍💼 admin.jsp

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
			<h1>ADMIN</h1>
		</main>
		<%@include file="/resources/layouts/footer.jsp" %>
	</div>
</body>
</html>
```

### 🧠 설명

- 관리자 전용 메인 페이지
    
- 외부 템플릿 구성 요소들을 include하여 일관된 레이아웃 구성
    
    - `link.jsp`, `topHeader.jsp`, `nav.jsp`, `footer.jsp`
        
- 본문 영역에서는 `<h1>ADMIN</h1>` 텍스트만 출력됨
    

---

## 📝 create.jsp (회원가입)

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
			<h1>USER JOIN</h1>
			<form action="${pageContext.request.contextPath}/user/create" method="post">
				USERNAME : <input name="username" /><br/>
				PASSWORD : <input name="password" /><br/>
				<button>회원가입</button>
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

- `/user/create` POST 요청을 처리하는 회원가입 폼
    
- 입력 필드: `username`, `password`
    
- 서버에서 검증 실패 시, `username_err` 메시지가 아래쪽 `<div>`에 출력됨
    
- 폼 action은 contextPath 기준으로 작성되어 배포 환경에서도 유연하게 동작
    

---

## 🔐 login.jsp

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
			<h1>USER LOGIN</h1>
			<form action="${pageContext.request.contextPath}/user/login" method="post">
				USERNAME : <input name="username" /><br/>
				PASSWORD : <input name="password" /><br/>
				<button>로그인</button>
			</form>
			<div>
				${username_err}
				${message}
			</div>
		</main>
		<%@include file="/resources/layouts/footer.jsp" %>
	</div>
</body>
</html>
```

### 🧠 설명

- 로그인 폼 페이지
    
- 로그인 실패 시, 유효성 오류 메시지(`username_err`) 또는 인증 실패 메시지(`message`) 출력
    
- 로그인 성공 시 서버에서는 세션에 사용자 정보 저장하고 리다이렉트
    

---

## ✅ 공통 사항

- 모두 `UTF-8`로 인코딩, JSP 상단에 선언되어 있음
    
- `pageContext.request.contextPath` 사용 → 경로 문제 방지
    
- `topHeader`, `nav`, `footer`, `link` 포함으로 레이아웃 통일
    
- 메시지 표현에는 EL (`${}`)을 사용
    

---
