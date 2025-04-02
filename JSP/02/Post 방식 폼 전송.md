# 📄 POST 방식 폼 전송 예제 (JSP Form with POST Method)

## 📌 개념 요약

`method="post"`를 사용해 사용자 입력값을 보다 안전하게 JSP로 전송하는 방식

---

## ✅ 주요 내용

- `form` 태그의 `method="post"`는 요청 데이터를 **본문(body)**에 담아 전송
    
- GET 방식과 달리 URL에 파라미터가 노출되지 않아 보안에 더 유리
    
- 로그인, 회원가입, 개인정보 입력 등에 POST 방식 사용
    
- `action="./C02Request_Process.jsp"`는 데이터를 처리할 JSP 경로 지정
    

---

## 💻 코드 예시 (HTML Form)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<!-- POST 방식으로 데이터 전송 -->
	<form action="./C02Request_Process.jsp" method="post">
		<!-- 사용자 이름 입력 -->
		<input type="text" name="username" /><br/>

		<!-- 비밀번호 입력 -->
		<input type="text" name="password"/><br/>

		<!-- 배경색 입력 -->
		<input type="text" name="bgcolor"/><br/>

		<!-- 전송 버튼 -->
		<button>전송</button>
	</form>

</body>
</html>
```

---

## 📌 정리

- POST 방식은 요청 데이터를 숨겨서 전송 → 보안에 유리
    
- URL에 파라미터 노출되지 않음
    
- JSP에서는 GET/POST 방식과 상관없이 `request.getParameter()`로 값 수신 가능
    

---

## 🔎 추가 개념

- **GET과 POST의 차이 요약**
    
    - GET: 빠름, 즐겨찾기/공유 가능, 길이 제한 있음, URL에 데이터 노출
        
    - POST: 보안에 강함, 길이 제한 없음, URL에 노출되지 않음
        
- **보안 처리**
    
    - POST 방식도 결국은 평문 전송 → 민감 정보는 HTTPS로 전송해야 안전
        
- **추천 상황**
    
    - GET: 검색, 페이지 이동
        
    - POST: 로그인, 데이터 저장, 파일 업로드 등