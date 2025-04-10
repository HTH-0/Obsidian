# C01Servlet_Test.java

## 📌 클래스 개요

- `javax.servlet.http.HttpServlet`을 상속받는 **서블릿 클래스**
    
- Servlet 생명주기 메서드(`init`, `service`, `destroy`)를 오버라이딩하여 **호출 흐름 확인 테스트용**
    

---

## 🔖 어노테이션

```java
@WebServlet("/TEST_01")
```

- 해당 서블릿은 `/TEST_01` 경로로 요청이 들어올 때 실행됨
    
- `web.xml`에 별도 설정 없이 어노테이션 기반으로 매핑
    

---

## 🧠 오버라이딩 메서드

### 1. `init()`

```java
@Override
public void init() throws ServletException {
	System.out.println("INIT() invoke...");
}
```

- 서블릿 최초 로딩 시 1회만 호출됨 (서버 시작 시 or 최초 요청 시)
    
- 리소스 초기화 등에 사용
    

---

### 2. `service(ServletRequest, ServletResponse)`

```java
@Override
public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
	System.out.println("SERVICE() invoke...");
}
```

- 클라이언트 요청이 들어올 때마다 호출됨
    
- **GET/POST 구분 없이 처리**하려는 경우 이 메서드 오버라이딩
    
- 일반적으로는 `doGet()` / `doPost()`를 오버라이딩하는 경우가 많음
    

---

### 3. `destroy()`

```java
@Override
public void destroy() {
	System.out.println("DESTROY() invoke...");
}
```

- 서블릿이 종료될 때 호출됨 (서버 종료 시 or 서블릿 언로드 시)
    
- 자원 해제 또는 정리 작업 수행
    

---

## 🧪 실행 흐름 요약

1. 서블릿 최초 요청 시 `init()` 1회 호출
    
2. 클라이언트 요청마다 `service()` 반복 호출
    
3. 서버 종료 시 `destroy()` 호출
    

```
→ INIT() invoke...
→ SERVICE() invoke...
→ DESTROY() invoke...
```

---

## ✅ 핵심 정리

- 서블릿 생명주기 메서드의 호출 순서를 테스트하기 위한 기본 구조
    
- 실습 또는 디버깅용으로 적합한 템플릿 코드
    
- 실무에서는 `doGet()` / `doPost()`로 요청을 구분해서 처리하는 방식이 더 자주 사용됨
    

---
