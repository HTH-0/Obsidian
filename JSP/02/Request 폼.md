# 📄 폼 데이터 전송 예제 (JSP Form Request)

## 📌 개념 요약

HTML `<form>`을 사용하여 사용자 입력값을 JSP로 전달하는 기본 예제

---

## ✅ 주요 내용

- `form` 태그는 데이터를 서버(JSP)에 전송하는 역할
    
- `action="./C01Request_Process.jsp"`: 데이터를 전송할 JSP 파일 지정
    
- `method="get"`: GET 방식으로 데이터 전송 (주소창에 데이터 노출됨)
    
- `<input name="...">`: 사용자의 입력값을 전달할 필드
    
- `name` 속성은 서버에서 해당 값을 식별하는 키로 사용됨
    

---

## 💻 코드 예시 (JSP Form)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<form action="./C01Request_Process.jsp" method="get">
		<input type="text" name="username" /><br/>
		<input type="text" name="password"/><br/>
		<input type="text" name="bgcolor"/><br/>
		<button>전송</button>
	</form>

</body>
</html>
```

---

## 📌 정리

- `<form>` 태그는 JSP와 클라이언트를 연결해주는 데이터 통로
    
- `name` 속성은 서버에서 데이터를 받을 때 key 값으로 사용
    
- GET 방식은 URL로 데이터 전송 (예: `...?username=홍길동&password=1234`)
    

---

## 🔎 추가 개념

- **GET vs POST 차이점**
    
    - GET: 빠르고 간단하지만 보안에 취약 (URL에 노출)
        
    - POST: 보안에 유리 (본문에 데이터 포함), 로그인·회원가입 등에 주로 사용
        
- `action` 경로는 JSP 위치 기준 상대경로 또는 절대경로 사용 가능
    

---

```dataviewjs
dv.pagePaths()
  .filter(p => p.includes("폼") || p.includes("form") || p.includes("Request"))
```