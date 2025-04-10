# 🌐 C01Filter_Test.java 필터 정리

## 📌 클래스 개요

- `javax.servlet.Filter` 인터페이스를 구현한 서블릿 필터 클래스
    
- 서블릿 실행 전/후에 특정 로직을 삽입할 수 있음
    
- 주로 **인증 처리, 로깅, 인코딩 설정, 접근 제어** 등에 사용됨
    

---

## 🔧 주요 구성

### 1. 필터 클래스 선언

```java
public class C01Filter_Test implements Filter
```

- 필터는 반드시 `Filter` 인터페이스 구현 필요
    
- 주요 메서드: `doFilter()`
    

---

### 2. 필터 적용 대상 (주석 처리됨)

```java
//@WebFilter("/index.do")
```

- 주석이 풀리면 `/index.do` 경로에 접근할 때 이 필터가 실행됨
    
- 어노테이션으로 간단히 적용 가능하며, `web.xml`로도 설정 가능
    

---

### 3. 핵심 메서드: `doFilter`

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
```

#### 📍 역할 설명

|위치|코드|역할|
|---|---|---|
|**요청 전**|`System.out.println("[FILTER] INDEX FILTER START...");`|요청 필터링 시작 (사전 처리)|
|**요청 처리**|`chain.doFilter(req, resp);`|다음 필터 or 서블릿으로 요청 전달|
|**응답 후**|`System.out.println("[FILTER] INDEX FILTER END...");`|요청 처리 후 응답 직전 후처리|

---

## 🧪 동작 예시 (콘솔 출력 흐름)

```
→ 클라이언트 → /index.do 요청
[FILTER] INDEX FILTER START...
→ Home 서블릿 실행
[FILTER] INDEX FILTER END...
```

---

## ✅ 활용 예시

|목적|사용 예|
|---|---|
|로그인 여부 검사|세션에 사용자 정보 없으면 `/login.do`로 리다이렉트|
|인코딩 처리|`req.setCharacterEncoding("UTF-8")`|
|공통 로깅|요청 시간, URI, 사용자 IP 출력 등|
|관리자 권한 확인|role 체크 후 `/forbidden.jsp`로 이동|

---

## 💡 확장 팁

- 여러 URL에 적용하려면:
    

```java
@WebFilter(urlPatterns = {"/main.do", "/admin/*"})
```

- 필터 실행 순서를 조절하려면 `@WebFilter` 대신 `web.xml`에서 `<filter-mapping>`에 `order` 설정 활용
    

---
