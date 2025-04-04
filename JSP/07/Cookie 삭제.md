# 📄 JSP - 쿠키 삭제 처리 예제 (`setMaxAge(0)` 사용)

## 📌 개념 요약

JSP에서 특정 이름의 쿠키를 삭제하려면, **같은 이름과 경로(path)**를 가진 쿠키를 생성하고 유지 시간을 0초로 설정하여 클라이언트에게 다시 전달하면 됨

---

## ✅ 주요 내용

- `new Cookie(name, null)` → 같은 이름의 쿠키 생성 (값은 null 또는 빈 문자열)
    
- `cookie.setMaxAge(0)` → 쿠키를 즉시 만료시킴
    
- `cookie.setPath("/")` → 전체 애플리케이션 경로에 적용되도록 설정
    
- `response.addCookie(cookie)` → 클라이언트에게 쿠키 삭제 명령 전송
    
- 이후 `location.href='getCookie.jsp'`로 리다이렉트
    

---

## 💻 코드 예시 (쿠키 삭제 처리)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	String cookieName = request.getParameter("cookieName");
	System.out.println("cookieName : " + cookieName);

	if(cookieName != null) {
		// 동일 이름의 쿠키 생성 (삭제용)
		Cookie cookie = new Cookie(cookieName.trim(), null);
		cookie.setMaxAge(0); // 0초 → 즉시 삭제
		cookie.setPath("/"); // 경로 일치해야 삭제됨
		response.addCookie(cookie); // 클라이언트에 전달
	}

	// 삭제 후 알림 및 페이지 이동
	out.println("<script>alert('쿠키삭제완료!'); location.href='getCookie.jsp'; </script>");
	// 또는 response.sendRedirect("getCookie.jsp");
%>
```

---

## 📌 정리

- **쿠키 삭제 조건**
    
    - 이름과 경로가 기존 쿠키와 **정확히 일치**해야 함
        
    - `setMaxAge(0)` 설정 필수
        
- 삭제 후 바로 확인하려면 `response.sendRedirect()` 또는 JavaScript로 페이지 이동 필요
    

---

## 🔎 추가 개념

- 쿠키 삭제는 클라이언트(브라우저)에게 "이 쿠키 이제 없애라"는 명령을 **다시 전송**하는 구조
    
- `setPath("/")`는 **모든 URL 경로**에서 유효한 쿠키를 의미하며, 생성 시와 동일해야 삭제 가능
    
- `response.sendRedirect()`는 서버 사이드 리다이렉트
    
- `location.href`는 클라이언트 사이드(JavaScript) 리다이렉트
    

---