# 📄 JSP - 회원가입 유효성 검사 및 세션 저장 예제

## 📌 개념 요약

회원가입 폼에서 전달된 사용자 입력값에 대해 서버에서 유효성 검사를 수행하고, 모든 조건을 만족하면 세션에 인증 정보를 저장함

---

## ✅ 주요 내용

- `request.getParameter("key")`: 사용자가 폼에 입력한 값 가져오기
    
- 입력값 검증 후 `request.setAttribute()`로 에러 메시지를 설정
    
- `request.getRequestDispatcher(...).forward(...)`로 다시 `join_form.jsp`로 이동 (값 유지)
    
- 조건 만족 시 `session.setAttribute()`를 통해 로그인 상태 처리
    

---

## 💻 코드 예시 (join.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	String email = request.getParameter("email");
	String username = request.getParameter("username");
	String password = request.getParameter("password");
	String password2 = request.getParameter("password2");

	// 유효성 검사: 이메일
	if(!email.equals("test@gmail.com")){
		request.setAttribute("email_msg", "유효한 이메일을 작성해주세요");
		request.getRequestDispatcher("./join_form.jsp").forward(request, response);
		return;
	}

	// 유효성 검사: 아이디
	if(!username.equals("admin")){
		request.setAttribute("username_msg", "유효한 아이디를 작성해주세요");
		request.getRequestDispatcher("./join_form.jsp").forward(request, response);
		return;
	}

	// 유효성 검사: 비밀번호
	if(!password.equals("1234")){
		request.setAttribute("password_msg", "유효한 비밀번호를 작성해주세요");
		request.getRequestDispatcher("./join_form.jsp").forward(request, response);
		return;
	}

	// 유효성 검사: 비밀번호 확인
	if(!password2.equals(password)){
		request.setAttribute("password2_msg", "비밀번호가 일치하지 않습니다");
		request.getRequestDispatcher("./join_form.jsp").forward(request, response);
		return;
	}

	// 가입 완료 → 세션 저장
	session.setAttribute("isAuth", true);     // 로그인 상태 저장
	session.setAttribute("role", "ROLE");     // 역할 저장

	// 성공 메시지 후 리디렉션
	out.println("<script>alert('회원가입 완료! 메인페이지로 이동합니다'); location.href='main.jsp';</script>");
%>
```

---

## 📌 정리

- 입력값을 하나씩 검사하고 조건이 맞지 않으면 `setAttribute()`로 에러 메시지 설정 후 다시 폼으로 `forward`
    
- 모든 검증 통과 시 로그인 상태와 권한을 세션에 저장
    
- 마지막에는 자바스크립트로 알림 띄우고 메인 페이지로 이동
    

---

## 🔎 추가 개념

- 여러 조건을 한 번에 검사하려면 `boolean isValid = true`와 조건 누적 방식 사용 가능
    
- `password2`는 `password.equals(password2)`로 비교하는 것이 올바름
    
- 실제 환경에서는 이메일 중복 확인, 비밀번호 암호화 등이 필요
    

---