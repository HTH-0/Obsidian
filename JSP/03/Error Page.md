
---

# 📄 JSP 에러 페이지 설정 (`isErrorPage="true"` 사용)

## 📌 개념 요약

JSP에서 예외 발생 시 별도의 에러 페이지로 이동시켜 `exception` 객체를 통해 에러 메시지를 출력할 수 있음.

---

## ✅ 주요 내용

- `isErrorPage="true"` : 해당 JSP가 에러 처리 전용 페이지임을 선언
    
- `exception` 객체 : 에러 페이지에서 자동으로 전달되는 내장 객체 (Exception 타입)
    
- 메인 JSP에서 `errorPage="에러페이지.jsp"` 설정 필요
    
- 사용자에게 친절한 에러 메시지를 제공하는 용도로 활용됨
    

---

## 💻 코드 예시

### 📎 main.jsp (에러 발생 가능 JSP)

```jsp
<%@ page errorPage="errorPage.jsp" %>
<%
    // 예외 강제 발생
    int result = 10 / 0;
%>
```

### 📎 errorPage.jsp (에러 처리 JSP)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" isErrorPage="true" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>에러 페이지</title>
</head>
<body>

    <h1>ERROR PAGE...</h1>
    <hr/>
    예외 메시지 : <%= exception.getMessage() %>

</body>
</html>
```

---

## 🧾 실행 결과 예시 (웹 브라우저 출력)

```
ERROR PAGE...
예외 메시지 : / by zero
```

---

## 📌 정리

- `isErrorPage="true"` 설정된 JSP에서는 `exception` 내장 객체 사용 가능
    
- 메인 JSP에서는 `errorPage="..."` 속성으로 에러 처리 페이지 연결
    
- 에러 처리 JSP는 사용자 친화적인 메시지로 구성할 수 있음
    

---

## 🔎 추가 개념

- `exception.getClass().getName()` → 예외 클래스 이름 출력 가능
    
- 전체 애플리케이션 공통 에러 처리를 위해 `web.xml`의 `<error-page>` 태그 설정 가능
    
- 자바스크립트 alert나 redirect로도 간단한 오류 알림 가능하나, 서버 오류는 서버에서 처리해야 함
    

---
