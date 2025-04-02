# 📄 request 내장객체로 폼 데이터 처리 (JSP Parameter Handling)

## 📌 개념 요약

`request.getParameter()`를 사용해 HTML 폼에서 전송된 데이터를 JSP에서 받아오는 기본 처리 방식

---

## ✅ 주요 내용

- `request`는 JSP의 내장 객체 중 하나로, 클라이언트가 보낸 요청 정보를 담고 있음
    
- `getParameter("파라미터명")`: HTML 폼에서 입력된 값을 문자열로 반환
    
- 전달받은 데이터를 활용하여 동적 출력 또는 스타일 적용 가능
    
- 삼항 연산자를 사용하여 기본 배경색 지정 가능 (예: 값이 없으면 회색)
    

---

## 💻 코드 예시 (C01Request_Process.jsp)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	// request 내장 객체로 클라이언트 입력값 받기
	String username = request.getParameter("username");
	String password = request.getParameter("password");
	String bgColor = request.getParameter("bgcolor");

	System.out.printf("%s , %s , %s \n", username, password, bgColor);
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>

<!-- 입력값이 없으면 기본 회색 배경 적용 -->
<body style="background-color:<%= bgColor.equals("") ? "gray" : bgColor %>">

username : <%= username %> 
<hr/>
password : <%= password %>
<hr/>
bgcolor : <%= bgColor %>

</body>
</html>
```

---

## 📌 정리

- `request.getParameter()`는 HTML `<form>`의 `name` 속성과 연결됨
    
- GET 방식으로 전달된 쿼리 스트링의 값을 문자열로 가져옴
    
- 값을 활용해 배경색이나 출력 내용을 동적으로 구성 가능
    
- null 또는 빈 문자열에 대비한 기본값 설정이 중요 (삼항 연산자 사용)
    

---

## 🔎 추가 개념

- **주의점**
    
    - `getParameter()`는 무조건 문자열 반환 → 형변환 필요 시 `Integer.parseInt()` 등 사용
        
    - 빈 값(`""`) 체크와 null 체크는 함께 다루는 게 안전함
        
- **백엔드 디버깅**
    
    - `System.out.printf()`를 사용해 콘솔에서 서버 로그 확인 가능
        
    - 실제 웹 출력과 별도로 서버 내부 상태를 점검할 수 있음
        

---