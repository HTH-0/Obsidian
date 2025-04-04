# 📄 JSP - 로그인 처리 로직 정리 (검증 → 인증 → 세션 저장)

## 📌 개념 요약

사용자가 입력한 아이디와 비밀번호를 검증하고, 조건이 맞으면 세션에 인증 정보를 저장한 후 메인 페이지로 이동시키는 로그인 처리 로직

---

## ✅ 주요 내용

- `request.getParameter()`: 폼에서 전달된 값 가져오기
    
- 빈 값 검증: `isEmpty()` 활용 → 입력값 누락 시 다시 로그인 폼으로 `forward`
    
- 아이디/비밀번호 검증 후 실패 시, 각각 다른 메시지를 `request.setAttribute()`로 전달
    
- 로그인 성공 시:
    
    - `session.setAttribute("isAuth", true)`: 로그인 인증 처리
        
    - `session.setMaxInactiveInterval(30)`: 세션 유지 시간 설정 (30초)
        
    - 자바스크립트로 메인 페이지 이동 안내 후 `main.jsp`로 이동
        

---

## 💻 코드 예시 (login.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	String username = request.getParameter("username");
	String password = request.getParameter("password");

	// 1단계: 입력값 검증
	if(username.isEmpty()){
		request.setAttribute("username_msg", "username을 입력하세요");
	}
	if(password.isEmpty()){
		request.setAttribute("password_msg", "password를 입력하세요");
	}
	if(username.isEmpty() || password.isEmpty()){
		request.getRequestDispatcher("./login_form.jsp").forward(request, response);
		return;
	}

	// 2단계: 사용자 ID 확인 (DB 조회 대신 하드코딩)
	if(!username.equals("admin")){
		request.setAttribute("username_msg", "해당 ID는 존재하지 않습니다");
		request.getRequestDispatcher("./login_form.jsp").forward(request, response);
		return;
	}

	// 3단계: 패스워드 확인
	if(!password.equals("1234")){
		request.setAttribute("password_msg", "패스워드가 일치하지 않습니다");
		request.getRequestDispatcher("./login_form.jsp").forward(request, response);
		return;
	}

	// 4단계: 인증 성공 - 세션 정보 저장
	session.setAttribute("isAuth", true); // 로그인 상태 저장
	session.setAttribute("role", "ROLE_ADMIN"); // 사용자 역할 지정
	session.setMaxInactiveInterval(30); // 세션 유효 시간: 30초

	// 5단계: 로그인 성공 메시지 및 메인 페이지 이동
	out.println("<script>alert('로그인 성공! MAIN page로 이동합니다.'); location.href='main.jsp';</script>");
%>
```

---

## 📌 정리

- 폼 입력값 검증 → DB 정보와 비교 (지금은 하드코딩) → 성공 시 세션 저장
    
- 실패할 경우 `request.setAttribute()`로 메시지를 넘겨 `forward`
    
- 로그인 성공 시 `session.setAttribute("isAuth", true)`로 인증 처리
    
- `forward`는 `request` 속성 유지, `redirect`는 새 요청이라 속성 소멸됨 주의
    

---

## 🔎 추가 개념

- `session.setMaxInactiveInterval(int seconds)`로 로그인 유지 시간 설정 가능
    
- 세션 정보는 브라우저 종료, 혹은 시간 초과 시 삭제됨
    
- 보안 강화를 위해 실제 프로젝트에서는 비밀번호 암호화, DB 연동, 실패 카운트 제한 필요
    

---