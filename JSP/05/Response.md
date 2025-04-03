
---

# 📤 `response` 객체 활용 예제 분석

## ✅ 주요 목적

- `response` 객체의 다양한 응답 처리 기능 테스트
    
- Redirect, Error 처리, OutputStream/Writer 출력 기능 체험
    

---

## 🧩 전체 코드 기능별 분석

### 1️⃣ **페이지 리디렉션 (Redirect)**

```jsp
// response.sendRedirect("./02Request.jsp");
```

- 클라이언트에게 **다른 페이지로 이동하라고 지시**
    
- 브라우저 주소(URL)도 변경됨
    
- 주로 로그인 후 메인 페이지 이동 등에 사용됨
    

```http
Status: 302 Found
Location: /프로젝트경로/02Request.jsp
```

---

### 2️⃣ **에러 응답 처리 (`sendError`)**

```jsp
// response.sendError(HttpServletResponse.SC_NOT_FOUND, "해당 페이지를 찾을 수 없습니다");
```

- 클라이언트에게 **특정 에러 상태 코드**를 반환함
    
- 일반적인 예:
    
    - `SC_NOT_FOUND` → 404 Not Found
        
    - `SC_FORBIDDEN` → 403 Forbidden
        
    - `SC_BAD_GATEWAY` → 502 Bad Gateway
        
- 커스텀 메시지를 함께 제공할 수 있음
    

💡 브라우저는 이 상태 코드를 받아 자체 에러 페이지를 표시하거나, 서버의 에러 페이지로 이동함.

---

### 3️⃣ **자동 새로고침 (Refresh 헤더 설정)**

```jsp
// response.setIntHeader("Refresh", 3);
```

- 클라이언트 브라우저에 **3초 후 자동 새로고침** 명령을 전달
    
- 주로 서버 응답 후 대기 페이지 구성 시 사용
    

---

### 4️⃣ **바이너리 출력 스트림 사용**

```jsp
/*
ServletOutputStream bout = response.getOutputStream();
bout.write('a');
bout.write(98); // 문자 'b'
bout.flush();
bout.close();
*/
```

- `ServletOutputStream`을 사용하여 **바이너리 데이터를 출력**
    
- HTML이 아닌 이미지, 파일 등도 전송 가능
    
- `getOutputStream()`과 `getWriter()`는 **동시에 사용 불가**
    

---

### 5️⃣ **텍스트 출력 (PrintWriter 사용)**

```jsp
PrintWriter o = response.getWriter();
o.write("<h1>HELLO WORLD</h1>");
```

- **HTML 텍스트 출력**을 위해 `getWriter()` 사용
    
- `PrintWriter`는 문자열 기반 텍스트 전송 전용
    
- 이 예제의 최종 출력 결과는:
    

```html
<h1>HELLO WORLD</h1>
```

---

### 6️⃣ **날짜 출력 (Expression 사용)**

```jsp
<%= new Date() %>
```

- 자바의 `Date` 객체를 HTML에 직접 출력
    
- 결과 예: `Thu Apr 03 10:41:15 KST 2025`
    

---

## 📌 요약 정리

|기능|설명|메서드|
|---|---|---|
|페이지 이동|클라이언트에게 새 페이지 요청하게 함|`sendRedirect(String url)`|
|에러 응답|지정된 HTTP 에러 코드 전송|`sendError(int code, String msg)`|
|새로고침|특정 초 후 자동 새로고침|`setIntHeader("Refresh", seconds)`|
|이진 출력|파일, 이미지 등 바이너리 응답|`getOutputStream()`|
|문자열 출력|HTML 응답 작성|`getWriter()`|

---

## ⚠️ 주의 사항

- `response.getWriter()` 와 `response.getOutputStream()`은 **둘 중 하나만 사용해야 함**
    
    - 둘 다 호출하면 **IllegalStateException 발생**
        
- `sendRedirect()`나 `sendError()`를 호출한 뒤에는 **응답이 종료되므로 HTML 출력 X**
    
- Content-Type 설정이 없으면 기본은 `text/html`임  
    → 명시적으로 설정하는 것이 좋음
    

```jsp
<%@ page contentType="text/html; charset=UTF-8" %>
```

---

## ✅ 추천 실습

- `sendRedirect()` 호출 후 `getWriter()` 사용 시 어떤 문제가 발생하는지 테스트해보기
    
- 브라우저 F12 → **Network 탭**에서 응답 코드 및 헤더 확인해보기
    
- `getOutputStream()`으로 이미지 출력해보는 바이너리 응답 예제 구성하기
    

---
