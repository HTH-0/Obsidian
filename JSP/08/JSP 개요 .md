# 🚀 서블릿(Servlet) & JSP 총정리

---

## 📌 개념 요약

- JSP는 Java Servlet 기반의 웹 페이지로, 서블릿을 HTML에 편리하게 통합한 기술
    

---

## ✅ 주요 내용

### JSP 주요 문법 구조

- **선언문** `<%! %>`  
    클래스의 속성이나 메서드를 정의할 때 사용. (Servlet의 멤버 영역에 포함됨)
    
- **스크립틀릿** `<% %>`  
    Java 코드를 넣는 공간으로, Servlet의 `service()` 메서드 안에 위치하게 됨.
    
- **표현식** `<%= %>`  
    변수 또는 연산 결과를 출력하며, 내부적으로 `out.print()`로 변환됨.
    
- **지시자** `<%@ %>`  
    JSP 페이지 설정을 지정하는 기능 (page 설정, import 클래스, include 등)
    
    ```jsp
    <%@ page import="java.util.Date" %>
    <%@ include file="header.jsp" %>
    ```
    

---

### 페이지 이동 방법 (Forward/Redirect)

|방식|Forward|Redirect|
|---|---|---|
|이동|서버 내 다른 페이지 이동|다른 페이지로 이동 유도|
|request 유지 여부|유지됨 (같은 요청 유지)|새로운 요청 생성|
|주요 메서드|`request.getRequestDispatcher().forward()`|`response.sendRedirect()`|

---

### 내장 객체 (Implicit Objects)

- JSP에서 자동 생성 및 사용 가능한 객체
    

|객체명|용도|
|---|---|
|`request`|클라이언트 요청 정보 및 매개변수 접근|
|`response`|클라이언트에게 응답 결과 전송|
|`session`|사용자 별로 상태 정보 저장|
|`application`|웹 애플리케이션 전역에서 정보 공유|
|`pageContext`|페이지 관련 정보를 제공, 경로 가져올 때 유용|

> `${pageContext.request.contextPath}`는 프로젝트 경로를 나타냄.

---

### 세션(Session)과 쿠키(Cookie)

|구분|세션(Session)|쿠키(Cookie)|
|---|---|---|
|저장 위치|서버|클라이언트(브라우저)|
|지속성|서버 종료 시 제거|브라우저에서 유지 (maxAge로 설정)|
|공통점|임시로 정보 저장 가능|임시로 정보 저장 가능|

---

### EL(Expression Language)

- JSP 표현식(`<%= %>`)을 간단하게 대체하기 위한 표현언어
    
- 주로 데이터 출력 시 사용됨
    

```jsp
${user.name} <!-- JSP 표현식인 <%= user.getName() %>를 대체 -->
```

---

### JSTL(JSP Standard Tag Library)

- JSP에서 자주 사용하는 기능을 표준 태그로 제공하는 라이브러리
    
- 반복, 조건문, 포맷팅, DB 연동 등 다양한 태그 제공
    

```jsp
<!-- JSTL 예시 -->
<c:if test="${user != null}">
    <p>안녕하세요, ${user.name}님!</p>
</c:if>
```

---

### DB 연결 흐름 (MVC 패턴 기준)

```
JSP(View) ↔ Controller(Servlet) ↔ Service ↔ DAO ↔ Database
```

---

## 💻 코드 예시

### JSP 기본 코드 예시

```jsp
<%@ page import="java.util.Date" %>
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JSP 예제</title>
</head>
<body>
  <h1>JSP 기본 구조</h1>

  <!-- 선언문 (속성, 메서드 선언 가능) -->
  <%! String message = "안녕하세요!"; %>

  <!-- 스크립틀릿 (Java 로직 수행 가능) -->
  <% 
    Date now = new Date();
    request.setAttribute("currentTime", now);
  %>

  <!-- 표현식 (출력 용도로 사용) -->
  <p>메시지: <%= message %></p>
  <p>현재 시간: <%= request.getAttribute("currentTime") %></p>
</body>
</html>
```

### 페이지 이동 예시

```java
// Forward
request.getRequestDispatcher("target.jsp").forward(request, response);

// Redirect
response.sendRedirect("target.jsp");
```

---

## 📌 정리

- **선언문**은 클래스 영역, **스크립틀릿/표현식**은 메서드 영역에서 실행됨.
    
- Forward는 요청 정보 유지, Redirect는 새로운 요청 생성.
    
- 세션은 서버에서 관리되고, 쿠키는 클라이언트에 저장되어 지속성이 다름.
    
- EL과 JSTL로 JSP 코드를 더 간결하고 가독성 높게 작성 가능.
    

---

## 🔎 추가 개념 (헷갈리기 쉬운 주의사항)

- **Forward vs Redirect**  
    로그인 처리 후 페이지 이동 시 보통 Redirect를 사용 (새로운 요청 필요)
    
- **쿠키 사용 시**  
    민감 정보 저장 금지 (클라이언트에서 접근 가능)
    
- **세션과 쿠키 차이**  
    세션은 서버 리소스를 소비, 쿠키는 클라이언트 자원을 소비
    
- **EL 사용 시 주의**  
    null 값 처리 시 JSTL의 `<c:if>` 태그 등을 활용하는 것이 좋음
    

---