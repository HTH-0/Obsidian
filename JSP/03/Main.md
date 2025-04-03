
---

# 📄 JSP에서 입력값 검증 및 에러 페이지 처리 (`errorPage` 사용)

## 📌 개념 요약

JSP에서 `request.getParameter()`로 받은 입력값을 검증하고, 조건에 따라 `Exception`을 발생시켜 지정된 에러 페이지로 전환할 수 있음.

---

## ✅ 주요 내용

- `errorPage="에러페이지.jsp"` : 예외 발생 시 이동할 JSP 지정
    
- `request.getParameter("key")` 로 폼 값 수신
    
- `isEmpty()` 검사를 통해 필수 입력값 체크
    
- 값이 비어 있으면 `throw new Exception(...)`으로 예외 발생
    
- 에러 페이지에서는 `exception.getMessage()`로 메시지 출력
    

---

## 💻 코드 예시

### 📎 01Form.jsp (폼 입력 페이지)

```jsp
<form action="02Main.jsp">
    <input type="text" name="name" placeholder="이름 입력"/><br>
    <input type="text" name="age" placeholder="나이 입력"/><br>
    <input type="text" name="addr" placeholder="주소 입력"/><br>
    <button>전송</button>
</form>
```

### 📎 02Main.jsp (입력값 처리 및 예외 발생)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="02Error.jsp" %>
<%
    String name = request.getParameter("name");
    String age = request.getParameter("age");
    String addr = request.getParameter("addr");

    // 입력값이 비었는지 검사하고 예외 발생
    if(name.isEmpty())
        throw new Exception("이름을 입력하세요");
    if(age.isEmpty())
        throw new Exception("나이를 입력하세요");
    if(addr.isEmpty())
        throw new Exception("주소를 입력하세요");
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>입력 결과</title>
</head>
<body>
    이름 : <%= name %><br>
    나이 : <%= age %><br>
    주소 : <%= addr %><br>
</body>
</html>
```

### 📎 02Error.jsp (에러 처리 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>에러 발생</title>
</head>
<body>

    <h1>입력 오류 발생</h1>
    <hr/>
    <p style="color:red;">
        오류 메시지 : <%= exception.getMessage() %>
    </p>

</body>
</html>
```

---

## 🧾 실행 흐름

1. `01Form.jsp`에서 입력 후 전송
    
2. `02Main.jsp`에서 입력값을 가져와 `isEmpty()`로 체크
    
3. 값이 비어 있으면 예외 발생 → `02Error.jsp`로 이동
    
4. 에러 페이지에서 예외 메시지 출력
    

---

## 📌 정리

- `errorPage="..."` 속성은 JSP 예외 처리를 위한 핵심 기능
    
- 입력 검증은 서버 측에서도 필수로 수행해야 함
    
- 에러 페이지에서는 `isErrorPage="true"` 선언 필요
    
- 사용자에게 안내할 수 있는 친절한 메시지 구성 중요
    

---

## 🔎 추가 개념

- `.isEmpty()`는 NullPointerException 발생 가능 → `null` 체크와 함께 쓰는 것이 안전
    

```java
if(name == null || name.isEmpty())
```

- HTML에서도 `required` 속성으로 1차 검증 가능 (클라이언트 측 유효성 검사)
    
- 서버 유효성 검사는 보안 및 데이터 무결성을 위해 항상 필요
    

---