# 🌐 `web.xml` 설정 파일 정리

## 📌 기본 설명

- `web.xml`은 **웹 애플리케이션의 배치 정보(Deployment Descriptor)**를 정의하는 파일
    
- `WEB-INF` 폴더 하위에 존재하며, **서블릿 매핑**, **초기 설정**, **웰컴 페이지 지정** 등 다양한 역할을 수행함
    
- 이번 설정은 프로젝트 이름이 `04SERVLET_INIT`인 JSP/Servlet 프로젝트의 설정 정보
    

---

## 🧾 전체 구성 요약

### 🔷 1. 프로젝트 기본 정보

```xml
<display-name>04SERVLET_INIT</display-name>
```

- 이 웹 애플리케이션의 이름을 정의
    
- 톰캣 서버 콘솔 또는 로그 상에 표시됨
    

---

### 🔷 2. 웰컴 파일 리스트

```xml
<welcome-file-list>
  <welcome-file>index.html</welcome-file>
  <welcome-file>index.htm</welcome-file>
  <welcome-file>index.jsp</welcome-file>
  <welcome-file>default.html</welcome-file>
  <welcome-file>default.htm</welcome-file>
  <welcome-file>default.jsp</welcome-file>
</welcome-file-list>
```

- 클라이언트가 `/`로 접속했을 때 가장 먼저 보여줄 **기본 페이지 순서**를 지정
    
- `index.jsp`가 가장 일반적
    
- 첫 번째로 존재하고 실제 파일이 존재하면 그 파일이 실행됨
    

---

### 🔷 3. 서블릿 등록 (클래스 지정)

```xml
<servlet>
  <servlet-name>C02Servlet</servlet-name>
  <servlet-class>Servlet.C02Servlet_Test</servlet-class>
</servlet>
```

- `C02Servlet_Test` 클래스는 어노테이션 없이 등록된 서블릿이므로 반드시 `web.xml`에 명시해야 동작
    
- `<servlet-name>`은 고유한 이름 (맘대로 정할 수 있음)
    
- `<servlet-class>`는 실제 서블릿 클래스의 패키지 경로
    

---

### 🔷 4. 서블릿 매핑 (URL 연결)

```xml
<servlet-mapping>
  <servlet-name>C02Servlet</servlet-name>
  <url-pattern>/TEST_02</url-pattern>
</servlet-mapping>
```

- 위에서 지정한 `C02Servlet` 이름을 `/TEST_02` URL과 매핑
    
- 클라이언트가 `/TEST_02`로 요청하면 `C02Servlet_Test`가 실행됨
    

---

## ✅ 핵심 정리

|기능|설명|
|---|---|
|`display-name`|프로젝트 이름 설정|
|`welcome-file-list`|최초 로딩 시 열리는 기본 페이지 파일 목록|
|`servlet`|서블릿 클래스 등록 (어노테이션 안 쓴 경우 필수)|
|`servlet-mapping`|서블릿 이름과 URL을 연결|

---

## 🔍 사용된 서블릿 비교

|서블릿 클래스|매핑 방식|URL 패턴|
|---|---|---|
|`C02Servlet_Test`|web.xml 수동 매핑|`/TEST_02`|
|`C01Servlet_Test` ~ `C04Servlet_Test`|어노테이션 기반 (`@WebServlet`)|각각의 경로 (`/main.do`, `/join.do` 등)|

---
