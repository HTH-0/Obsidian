
---

# 🧪 `pageContext` 활용 예제 분석

## ✅ 역할 요약

- `pageContext` 객체를 이용해 **내장 객체 접근**과 **프로젝트 루트 경로 출력**
    
- 서버 콘솔 로그와 클라이언트 HTML 모두에 정보를 출력
    

---

## 📌 주요 코드 분석

### 🔹 콘솔 출력 (`System.out.println`)

```jsp
System.out.println("pageContext : " + pageContext);
System.out.println("pageContext's get request : " + pageContext.getRequest());
System.out.println("pageContext's get response : " + pageContext.getResponse());
System.out.println("pageContext's get session : " + pageContext.getSession());
System.out.println("pageContext's get application : " + pageContext.getServletContext());
System.out.println("project path : " + pageContext.getServletContext().getContextPath());
```

- 위 코드는 **서버 콘솔**에 출력됨 (브라우저에서는 안 보임)
    
- `pageContext`로부터 다음 객체에 접근:
    
    - `request` → `HttpServletRequest`
        
    - `response` → `HttpServletResponse`
        
    - `session` → `HttpSession`
        
    - `application` → `ServletContext`
        
- 마지막 줄은 프로젝트의 **ContextPath** 출력 (예: `/MyWebProject`)
    

---

### 🔹 표현식(Expression)

```jsp
PROJECTPATH : <%=pageContext.getServletContext().getContextPath()%>
```

- 브라우저에 출력되는 부분
    
- `pageContext` → `ServletContext` → `getContextPath()` 흐름
    
- 결과 예시:
    
    ```
    PROJECTPATH : /mywebapp
    ```
    

---

### 🔹 EL(Expression Language)

```jsp
PROJECTPATH(EL) : ${ pageContext.request.contextPath }
```

- EL을 사용한 같은 내용 출력
    
- EL에서는 `pageContext`를 통해 `request` 접근 가능
    
- EL 버전은 더 깔끔하고 유지보수 쉬움
    

---

## 📌 요점 정리

|사용|표현 방식|결과|
|---|---|---|
|자바 표현식|`<%= ... %>`|HTML에 직접 출력|
|EL|`${ ... }`|표현식보다 간결, 추천|
|콘솔 로그|`System.out.println()`|서버 콘솔에 출력됨, 디버깅용|

---

## 🔎 부가 설명: `getContextPath()`란?

- `getContextPath()`는 현재 웹 애플리케이션의 **루트 경로**를 반환함
    
- 예를 들어, 다음과 같은 주소에서:
    

```
http://localhost:8080/mywebapp/member/login.jsp
```

→ `getContextPath()`의 값은 `/mywebapp`

**왜 중요할까?**

- 이미지, CSS, JS 경로를 동적으로 처리할 때 유용
    

```jsp
<link rel="stylesheet" href="${pageContext.request.contextPath}/css/style.css">
```

---
