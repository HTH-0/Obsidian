# 📄 JSP - 회원가입 폼 (JOIN FORM) 구성 예제

## 📌 개념 요약

사용자의 회원가입 입력값을 받아 서버로 전송하고, 입력값 검증 메시지를 동적으로 표시할 수 있는 HTML+JSP 기반의 회원가입 폼

---

## ✅ 주요 내용

- `<form action="...">`: 회원가입 요청을 `join.jsp`로 전달
    
- `name="..."`: 각 입력 필드에 대응하는 파라미터 이름 지정
    
- `${param.message}`: URL 쿼리 파라미터로 전달된 메시지를 화면에 출력
    
- `${email_msg}`, `${username_msg}` 등: 유효성 검증 실패 시 서버에서 넘긴 에러 메시지를 EL로 표시
    

---

## 💻 코드 예시 (join_form.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원가입 폼</title>
</head>
<body>

<h1>JOIN FORM</h1>

<!-- 쿼리 파라미터 메시지 출력 -->
<div style="min-height: 25px; font-size: .8rem; color: orange">
	${param.message}
</div>

<!-- 회원가입 입력 폼 -->
<form action="${pageContext.request.contextPath}/C06/03/join.jsp" method="post">
	<div>
		<label>이메일 :</label><span>${email_msg}</span><br />
		<input type="text" name="email" />
	</div>
	<div>
		<label>아이디 :</label><span>${username_msg}</span><br />
		<input type="text" name="username" />
	</div>
	<div>
		<label>비밀번호 :</label><span>${password_msg}</span><br />
		<input type="text" name="password" />
	</div>
	<div>
		<label>비밀번호 확인 :</label><span>${password2_msg}</span><br />
		<input type="text" name="password2" />
	</div>
	<div>
		<button>회원가입</button>
	</div>
</form>

</body>
</html>
```

---

## 📌 정리

- `form`의 `name` 속성과 `request.getParameter("...")`는 1:1로 매칭
    
- `method="post"`를 반드시 지정해야 사용자 정보가 URL에 노출되지 않음
    
- 오류 메시지 출력용 `${xxx_msg}`는 `request.setAttribute("xxx_msg", "...")`로 전달되어야 정상 표시됨
    
- `${param.message}`는 `?message=...`처럼 URL 파라미터에서 자동 추출됨
    

---

## 🔎 추가 개념

- 입력값 검증은 서버 측 `join.jsp`에서 수행 (빈값, 비밀번호 일치 등)
    
- `request.getRequestDispatcher("join_form.jsp").forward()`를 사용하면 입력값 유효성 실패 시 다시 폼으로 돌아갈 수 있음
    
- 보안 및 사용자 편의를 위해 클라이언트 측 `JavaScript` 유효성 검사도 추가 권장
    

---