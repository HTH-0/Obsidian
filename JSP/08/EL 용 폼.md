# 📄 JSP 기본 입력 폼 예제 정리

## 제목(한글 + 영어): JSP 폼 입력 처리 (Form Input Handling in JSP)

## 📌 개념 요약

HTML `<form>`을 이용해 사용자 입력값을 전송하고, JSP에서 해당 값을 처리할 수 있도록 하는 기본 구조

---

## ✅ 주요 내용

- `form` 태그를 사용해 클라이언트가 입력한 데이터를 서버로 전송
    
- `action` 속성: 데이터를 전송할 대상 URL 지정
    
- `method` 기본값은 GET (데이터가 URL에 노출됨)
    
- `<input name="...">`에 지정한 이름으로 서버에서 데이터 추출 가능
    
- JSP 파일 맨 위에는 page 지시자를 통해 문자 인코딩 설정
    

---

## 💻 코드 예시 (JSP)

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
	<!-- 사용자 입력값을 ./result/jsp 경로로 전송 -->
	<form action="./result/jsp">
		<div>
			<!-- 사용자 이름 입력 필드 -->
			<input type="text" name="username" />
		</div>
		<div>
			<!-- 비밀번호 입력 필드 -->
			<input type="text" name="password" />
		</div>
		<div>
			<!-- 전송 버튼 -->
			<input type="submit" value="전송" />
		</div>
	</form>
</body>
</html>
```

---

## 📌 정리

- 입력 필드에 name 속성을 반드시 지정
    
- form의 action 경로에 전송할 JSP나 컨트롤러 지정
    
- 데이터는 GET 방식으로 URL에 노출되어 전송됨 (`?username=...&password=...`)
    
- 서버 측에서는 `request.getParameter("username")` 등으로 값 추출
    

---

## 🔎 추가 개념

- 보안상 패스워드 입력은 `type="password"`로 지정하는 것이 일반적
    
- `method="post"`를 사용하면 입력값이 URL에 노출되지 않음
    
- action 경로는 실제 URL 매핑과 일치해야 함 (`/result.jsp` 또는 컨트롤러 경로 등)
    

---