# 📄 JSP - 로그아웃 처리 로직 정리 (`session.invalidate()` 사용)

## 📌 개념 요약

`session.invalidate()` 메서드를 사용하면 현재 사용자의 세션을 완전히 종료시켜 로그아웃 처리 가능

---

## ✅ 주요 내용

- `session.invalidate()`: 세션에 저장된 모든 속성을 제거하고, 세션 자체를 무효화
    
- `out.println("<script>...")`: JavaScript를 통해 로그아웃 성공 메시지를 띄우고 로그인 페이지로 이동
    
- 쿼리 스트링에 `message=Logout_Successful` 추가 → 로그인 페이지에서 메시지 표시 가능
    

---

## 💻 코드 예시 (logout.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	// 세션 종료
	session.invalidate();

	// 로그아웃 후 로그인 페이지로 이동
	out.println("<script> alert('LOGOUT 성공! LOGIN PAGE로 이동합니다.'); location.href='./login_form.jsp?message=Logout_Successful'; </script>");
%>
```

---

## 📌 정리

- 로그아웃 처리는 `session.invalidate()`로 세션 통째로 삭제
    
- 이후 페이지 이동은 JavaScript의 `location.href`로 처리
    
- 로그인 폼에서 `${param.message}`를 통해 메시지 출력 가능
    

---

## 🔎 추가 개념

- `session.removeAttribute("key")`를 쓰면 특정 속성만 제거 가능
    
- 로그아웃 후에는 반드시 세션 검사를 다시 구현해야 보안 유지 가능
    
- 로그인 상태 확인용 조건 예시:
    
    ```jsp
    if(session.getAttribute("isAuth") == null){
        response.sendRedirect("login_form.jsp");
    }
    ```
    

---