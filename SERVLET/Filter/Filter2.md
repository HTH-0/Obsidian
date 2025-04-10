# 🧩 C02Filter_Test.java 필터 정리

## 📌 클래스 개요

- `Filter` 인터페이스를 구현한 서블릿 필터 클래스
    
- `/main.do` 요청에 대해 **전후처리 로직**을 삽입하는 예제
    
- 현재는 로그 출력만 수행 → **확장용 필터 템플릿**
    

---

## 🔧 기본 구조

```java
public class C02Filter_Test implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
        throws IOException, ServletException {
        System.out.println("[FILTER] MAIN FILTER  START...");
        chain.doFilter(req, resp);
        System.out.println("[FILTER] MAIN FILTER END...");
    }
}
```

---

## 🧷 적용 대상 URL

```java
//@WebFilter("/main.do")
```

- 주석 해제 시 `/main.do`로 요청 들어올 때 필터가 실행됨
    
- 주로 로그인 성공 후 들어오는 **메인 페이지 요청을 감시**할 때 사용
    

---

## 🧪 실행 흐름 예시 (콘솔 로그)

```
[FILTER] MAIN FILTER  START...
→ Home 서블릿 실행 (main.jsp forward)
[FILTER] MAIN FILTER END...
```

---

## ✅ 현재 역할

|구분|설명|
|---|---|
|요청 전 처리|콘솔에 시작 로그 출력|
|요청 전달|`chain.doFilter(req, resp)`로 서블릿 or 다음 필터로 전달|
|응답 후 처리|콘솔에 종료 로그 출력|

---

## 🛠️ 확장 예시 (로그인 상태 확인)

필터 본문을 아래처럼 확장하면 **비로그인 사용자 차단** 가능:

```java
HttpServletRequest request = (HttpServletRequest) req;
HttpServletResponse response = (HttpServletResponse) resp;
HttpSession session = request.getSession(false);

if (session == null || session.getAttribute("username") == null) {
    response.sendRedirect(request.getContextPath() + "/login.do");
    return;
}
chain.doFilter(req, resp);
```

---

## ✨ 활용 아이디어

|목적|사용 방법|
|---|---|
|로그인 필터|세션에 사용자 ID 없으면 `/login.do`로 리다이렉트|
|관리자 필터|세션의 `role` 값이 `ROLE_ADMIN`이 아닌 경우 차단|
|권한별 접근 제한|URL 패턴별 필터 다르게 적용|
|공통 로깅|모든 요청 정보 저장 (IP, 시간 등)|

---
