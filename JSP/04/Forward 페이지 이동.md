
---

# 🔄 Forward 방식 페이지 이동 (RequestDispatcher)

## 📌 개념 요약

서버 내부에서 요청을 다른 JSP나 서블릿으로 전달하는 방식 (요청 객체 공유)

---

## ✅ 주요 내용

- **Forward 방식**은 `RequestDispatcher`를 사용하여 **서버 내부에서 페이지를 이동**
    
- 클라이언트가 보는 **URL은 변하지 않음**
    
- 최초 요청 시 생성된 **request 객체가 유지됨**
    
- **페이지 이동은 서버 내부에서 처리**, 브라우저는 이동을 인식하지 못함
    
- **하나의 요청(request)과 응답(response) 사이클** 내에서 처리됨
    

---

## 💻 코드 예시

### 📄 `01Start.html`

사용자로부터 데이터를 입력받아 `02Page.jsp`로 전달

```html
<!-- 01Start.html -->
<form action="02Page.jsp">
    <input type="text" name="username" />
    <input type="text" name="password" />
    <button>전송</button>
</form>
```

---

### 📄 `02Page.jsp`

입력된 값을 확인하고, `03Page.jsp`로 Forward 이동

```jsp
<%
    String id = request.getParameter("username");
    String pw = request.getParameter("password");

    // RequestDispatcher를 이용한 forward 처리
    RequestDispatcher rd = request.getRequestDispatcher("03Page.jsp");
    rd.forward(request, response);
%>
```

---

### 📄 `03Page.jsp`

`02Page.jsp`에서 전달받은 데이터 출력 및 다음 페이지로 Forward

```jsp
<%
    String id = request.getParameter("username");
    String pw = request.getParameter("password");
%>

<p>아이디: <%= id %></p>
<p>비밀번호: <%= pw %></p>

<%
    RequestDispatcher rd = request.getRequestDispatcher("04Result.jsp");
    rd.forward(request, response);
%>
```

---

### 📄 `04Result.jsp`

최종 결과 출력 (계속해서 request 객체 사용 가능)

```jsp
<%
    String id = request.getParameter("username");
    String pw = request.getParameter("password");
%>

<h2>최종 확인</h2>
<p>ID: <%= id %></p>
<p>PW: <%= pw %></p>
```

---

## 📌 정리

- **RequestDispatcher.forward()**
    
    - 서버 내부에서 이동
        
    - `request`, `response` 유지
        
    - 클라이언트 URL 변경 없음
        
- **브라우저는 하나의 요청으로만 인식**
    
- form → JSP1 → JSP2 → JSP3 모두 같은 request 흐름
    

---

## 🔎 추가 개념

- `response.sendRedirect()`와 **반대 개념**
    
    - sendRedirect는 **새 요청**, request 객체 공유 불가
        
    - forward는 **같은 요청**, request 유지됨
        
- 포워드 체인을 너무 길게 구성하면 유지보수 어려움
    

---
