# 📄 JSP - request / session 속성 출력 예제 정리

## 📌 개념 요약

JSP에서는 `request`와 `session` 객체를 통해 서버에 저장된 데이터를 출력할 수 있음

---

## ✅ 주요 내용

- `request.getAttribute("key")`: 요청(request)에 저장된 속성(attribute) 값을 가져옴
    
    - 같은 요청 안에서만 유효 (페이지 이동 시 사라짐)
        
- `session.getAttribute("key")`: 세션(session)에 저장된 속성 값을 가져옴
    
    - 브라우저가 유지되는 동안(또는 세션이 만료될 때까지) 유지됨
        
- `<%= %>`: JSP 표현식(Expression)으로, 값을 HTML에 바로 출력할 때 사용
    

---

## 💻 코드 예시 (속성 값 출력 JSP)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>속성 출력 예제</title>
</head>
<body>
	
	<h1>속성 출력 확인</h1>
	
	<!-- request에 저장된 속성 출력 -->
	<div>REQUEST 속성 확인</div>
	ID : <%= request.getAttribute("ID1") %> <br />
	PW : <%= request.getAttribute("PW1") %> <br />
	
	<!-- session에 저장된 속성 출력 -->
	<div>SESSION 속성 확인</div>
	ID : <%= session.getAttribute("ID2") %> <br />
	PW : <%= session.getAttribute("PW2") %> <br />
	
</body>
</html>
```

📌 **예상 출력 예시 (값이 저장된 경우)**

```html
REQUEST 속성 확인  
ID : user123  
PW : pass123  

SESSION 속성 확인  
ID : user123  
PW : pass123  
```

---

## 📌 정리

- `request`: 한 번의 요청 동안만 유지 → 주로 `forward` 시 데이터 전달용
    
- `session`: 클라이언트와의 연결(세션) 동안 유지 → 로그인 정보 저장 등에 사용
    

---

## 🔎 추가 개념

- `request.setAttribute("ID1", "user123")`는 서버에서 이 페이지를 `forward`하기 전에 수행되어야 함
    
- `session.setAttribute("ID2", "user123")`는 세션이 존재하는 동안 어디서든 접근 가능
    
- `getParameter()`와 `getAttribute()`는 다름
    
    - `getParameter()`: form이나 쿼리 문자열을 통해 전달된 값을 가져옴 (항상 문자열)
        
    - `getAttribute()`: 자바 객체 자체를 저장하고 꺼낼 수 있음 (Object 타입)
        

---