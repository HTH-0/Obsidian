
---

# 📄 JSP에서 HTML 폼으로 데이터 전송하기 (`getParameter()` 사용)

## 📌 개념 요약

HTML `<form>`을 통해 사용자 입력을 전송하고, JSP에서 `request.getParameter()`로 값을 받을 수 있음.

---

## ✅ 주요 내용

- `<form action="...">` : 폼 데이터를 전송할 대상 JSP 지정
    
- `<input name="...">` : 각각의 입력값은 name 속성으로 식별
    
- `method` 기본값은 `GET` → URL에 데이터가 노출됨
    
- JSP에서는 `request.getParameter("name")`으로 해당 값 추출
    

---

## 💻 코드 예시

### 📎 01Form.jsp (입력 폼 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>입력 폼</title>
</head>
<body>

    <form action="02Main.jsp">
        <input type="text" name="name" placeholder="이름 입력"/><br>
        <input type="text" name="age" placeholder="나이 입력"/><br>
        <input type="text" name="addr" placeholder="주소 입력"/><br>
        <button>전송</button>
    </form>

</body>
</html>
```

### 📎 02Main.jsp (폼 데이터 처리 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>폼 결과</title>
</head>
<body>

    이름 : <%= request.getParameter("name") %> <br/>
    나이 : <%= request.getParameter("age") %> <br/>
    주소 : <%= request.getParameter("addr") %> <br/>

</body>
</html>
```

---

## 🧾 실행 흐름 요약

1. 사용자가 `01Form.jsp`에서 입력 후 [전송] 클릭
    
2. `02Main.jsp`로 값이 전달됨 (GET 방식)
    
3. `request.getParameter()`를 통해 각각의 값 출력
    

---

## 📌 정리

- `form` 전송 시 name 속성이 key, 입력값은 value
    
- `request.getParameter("key")` 로 개별 값 접근
    
- `GET` 방식은 URL에 값이 노출됨 → 중요한 정보는 `POST` 권장
    

---

## 🔎 추가 개념

- `method="post"`를 추가하면 보안성 향상 및 URL 깔끔하게 유지
    
- 숫자 입력을 받았더라도 `getParameter()`의 결과는 항상 String  
    → `Integer.parseInt()`로 형변환 필요
    
- form 전송 대상은 JSP뿐만 아니라 서블릿, 다른 웹 리소스도 가능
    

---
