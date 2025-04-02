# 📄 JSP 초기화 제어 및 JS 자동 폼 전송 예제

## 📌 개념 요약

JSP 내에서 `isInit` 플래그를 통해 최초 요청 여부를 제어하고, JS를 통해 조건부로 **폼을 자동 전송**하는 방식

---

## ✅ 주요 내용

- `<%! ... %>`: JSP 선언부, 전역 변수 정의 (`isInit`은 JSP 내 전역 변수로 동작)
    
- `flag` 파라미터로 재요청 여부를 판단해 `isInit` 값을 변경
    
- JS에서 `isInit` 값이 true일 때만 폼을 자동으로 채워서 전송
    
- **자동 리퀘스트-응답 루프를 구현**할 수 있음 (최초 요청 → 값 설정 후 재요청)
    

---

## 💻 코드 예시

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<%!
		// JSP 선언부 - 전역 변수
		boolean isInit = true;
	%>

	<%
		// 요청 파라미터 수신
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		String role = request.getParameter("role");
		String flag = request.getParameter("flag");

		System.out.println("FLAG : " + flag);
		System.out.println("isInit = " + isInit);

		// flag 값이 true면 isInit을 false로 바꿈
		if(flag != null && flag.equals("true")){
			isInit = false;
		}
	%>

	<!-- 출력 -->
	USERNAME : <%=username%><br>
	PASSWORD : <%=password%><br>
	ROLE : <%=role%><br>

	<!-- 자동 전송용 form (hidden 필드) -->
	<form action="C04JStoJSP.jsp" name="myForm">
		<input name="username" type="hidden" /> 
		<input name="password" type="hidden" /> 
		<input name="role" type="hidden" /> 
		<input name="flag" value="true" type="hidden" />
	</form>

	<script>
		const form = document.myForm;
		const flag = '<%=isInit%>';  // JSP 변수 → JS 문자열
		console.log("flag", flag);

		// 최초 요청일 경우 자동으로 값 설정 후 전송
		if(flag == 'true'){
			form.username.value = "홍길동";
			form.password.value = "1234";
			form.role.value = "ROLE_USER";
			form.submit();
		}
	</script>
	
</body>
</html>
```

---

## 📌 정리

- `<%! ... %>` 선언부는 JSP 페이지 전체에서 공유됨 (전역 변수 역할)
    
- `isInit` 값에 따라 JS가 동작하도록 제어 가능
    
- 폼 자동 전송은 로그인 시 자동 채움, 초기화 처리 등에 응용 가능
    
- JSP에서 JS로 서버 변수 값을 넘길 때는 `<%=변수%>`로 출력
    

---

## 🔎 추가 개념

- JSP 페이지는 요청이 들어올 때마다 새로 실행되므로, **선언부 변수도 서블릿 인스턴스 단위로 유지**됨
    
- 실제 서비스에서는 `isInit` 같은 제어는 **세션**, **쿠키**, **DB 상태** 등으로 관리하는 게 일반적
    
- 자동 폼 전송 시 `flag` 파라미터로 재전송 여부를 구분하면 무한 루프 방지 가능
    

---