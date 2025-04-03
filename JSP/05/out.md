# 🖨 JSP에서 `out` 객체를 사용한 출력 예제

## ✅ 역할 요약

JSP 내장 객체 `out`을 이용해 클라이언트 브라우저에 **HTML 콘텐츠를 출력**하는 기본 예제

---

## 💻 코드 분석

```jsp
<%
	out.write("<h1>HELLO WORLD</h1>");
%>
```

### 🔹 `out` 객체

- JSP의 기본 내장 객체 중 하나
    
- `javax.servlet.jsp.JspWriter` 타입
    
- 클라이언트(브라우저)로 **텍스트 출력**하는 데 사용
    
- `write()`, `print()`, `println()` 메서드 제공
    

---

## ✅ 출력 결과 (브라우저)

```html
<h1>HELLO WORLD</h1>
```

- 브라우저에서는 위 HTML 태그가 적용되어 굵은 큰 글씨로 “HELLO WORLD” 출력됨
    

---

## 📌 관련 메서드 차이

|메서드|설명|
|---|---|
|`out.print("text")`|문자열 출력|
|`out.println("text")`|문자열 출력 후 줄바꿈|
|`out.write("text")`|`Writer` 기반 출력, 고성능 (주로 내부 처리용)|

---

## ⚠️ 주의 사항

- `out`은 텍스트 출력 전용 → **바이너리 전송에는 부적합**
    
- `response.getWriter()`와 같은 역할이지만 JSP에선 자동 제공됨
    
- `response.getOutputStream()`과 **동시에 사용 불가** → 충돌 발생
    

---

## ✅ 참고

- `out.write()`는 서블릿에서 사용하는 `PrintWriter`의 write()와 유사하지만 JSP에서는 `JspWriter`로 감싸진 상태
    
- HTML 태그를 문자열로 감싸 출력하면 브라우저가 HTML로 해석함
    

---