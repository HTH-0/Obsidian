이번에는 업로드한 HTML 및 JSP 파일들을 기반으로 **Redirect 방식 페이지 이동**에 대한 개념과 흐름을 정리해줄게.

---

# 🔁 Redirect 방식 페이지 이동 (response.sendRedirect)

## 📌 개념 요약

서버가 클라이언트에게 새로운 요청 URL을 알려주고, 클라이언트가 다시 요청하는 방식

---

## ✅ 주요 내용

- 서버에서 **`response.sendRedirect("url")`**로 클라이언트에게 새 주소 요청을 지시
    
- 클라이언트는 새로운 URL로 **다시 요청**, **request/response 객체는 새로 생성됨**
    
- 브라우저의 **URL이 변경됨**
    
- **두 번의 요청-응답 주기**가 발생
    
- 페이지 이동 사실이 브라우저에 **노출됨**
    

---

## 💻 코드 예시

### 📄 `01Start.html`

사용자 입력을 받아 `02Page.jsp`로 전달 (GET 방식)

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

사용자 입력 확인 후, **Redirect 방식으로** `03Page.jsp`로 이동

```jsp
<%
    String id = request.getParameter("username");
    String pw = request.getParameter("password");

    // redirect 방식으로 이동
    response.sendRedirect("03Page.jsp?username=" + id + "&password=" + pw);
%>
```

---

### 📄 `03Page.jsp`

Redirect를 통해 전달받은 파라미터를 출력하고 다시 `04Result.jsp`로 Redirect

```jsp
<%
    String id = request.getParameter("username");
    String pw = request.getParameter("password");
%>

<p>아이디: <%= id %></p>
<p>비밀번호: <%= pw %></p>

<%
    // 다시 redirect로 결과 페이지로 이동
    response.sendRedirect("04Result.jsp?username=" + id + "&password=" + pw);
%>
```

---

### 📄 `04Result.jsp`

최종 확인 페이지, 전달받은 값 출력

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

- **response.sendRedirect("url")**
    
    - 클라이언트가 새로운 URL로 **재요청**
        
    - **request, response 객체는 새로 생성됨**
        
    - 브라우저 주소창 URL이 변경됨
        
- **데이터 전달 시 URL 파라미터 필요**
    
- **2번 요청 발생 → 느릴 수 있음**
    

---

## 🔎 추가 개념

- `forward`와의 비교
    
    |구분|forward|redirect|
    |---|---|---|
    |요청 횟수|1번|2번|
    |request 공유|O|X|
    |브라우저 URL 변경|X|O|
    |사용 방식|서버 내부 이동|클라이언트 재요청|
    
- 로그인 처리 후 메인 페이지 이동 등에서는 **redirect**를 자주 사용
    

---
