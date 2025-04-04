# 📄 JSP - 로그인 세션 검증 및 메인 페이지 구성 예제

## 📌 개념 요약

메인 페이지 진입 시 로그인 여부를 `session`으로 검사하고, 인증되지 않은 경우 로그인 페이지로 리다이렉트 처리

---

## ✅ 주요 내용

- `session.getAttribute("isAuth") == null`  
    → 로그인하지 않았거나 세션이 만료된 상태
    
- `response.sendRedirect("login_form.jsp?message=Session_Expired")`  
    → 새 요청으로 로그인 폼 이동하며 메시지 전달
    
- 인증된 경우에는 `session`에 저장된 사용자 정보(`isAuth`, `role`) 출력
    
- JSTL/EL 표현식 사용 시 `<c:out>` 없이 `${}`만으로 세션 속성 출력 가능
    

---

## 💻 코드 예시 (main.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	// 로그인 여부 확인
	if(session.getAttribute("isAuth") == null){
		// 세션 만료 또는 미로그인 상태 → 로그인 페이지로 이동
		response.sendRedirect("./login_form.jsp?message=Session_Expired");
		return; // 아래 HTML 출력 방지
	}
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>메인 페이지</title>
</head>
<body>

<!-- 로그아웃 링크 -->
<a href="./logout.jsp">로그아웃</a>
<hr/>

<h1>MAIN PAGE</h1>

<!-- 로그인 세션 정보 출력 -->
ISAUTH : <%= session.getAttribute("isAuth") %> <br/>
ISAUTH(EL): ${isAuth} <br/>

ROLE : <%= session.getAttribute("role") %> <br/>
ROLE(EL) : ${role} <br/>

</body>
</html>
```

---

## 📌 정리

- 로그인 여부는 `session.getAttribute("isAuth")`로 확인
    
- 인증되지 않으면 로그인 페이지로 `redirect` (새 요청 발생)
    
- 로그인된 사용자의 권한 정보는 `session`에서 직접 또는 `${}`로 출력 가능
    
- 리다이렉트 시 쿼리 파라미터로 메시지 전달 가능 → `${param.message}`로 출력
    

---

## 🔎 추가 개념

- `forward`를 쓰면 기존 request와 attribute 유지, `redirect`는 새 요청
    
- JSP에서 EL 표현식으로 세션 값 출력하려면, `session.setAttribute("key", value)`로 설정되어 있어야 함
    
- EL이 인식하는 범위: `page`, `request`, `session`, `application`
    

---