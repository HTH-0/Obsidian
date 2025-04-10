# 서블릿 생명주기 테스트 (C02Servlet_Test)

## 📌 클래스 개요

- `javax.servlet.http.HttpServlet`을 상속받는 서블릿 클래스
    
- 서블릿의 생명주기 메서드(`init`, `service`, `destroy`)를 오버라이딩하여 **기본 동작을 콘솔 로그로 확인**하는 실습용 서블릿
    
- URL 매핑 어노테이션이 없음 → **web.xml에서 수동 등록 필요**
    

---

## 🔧 주요 오버라이딩 메서드

### 1. `init()`

```java
@Override
public void init() throws ServletException {
	System.out.println("INIT() invoke...");
}
```

- 서블릿이 처음 로딩될 때 한 번만 호출됨
    
- 서블릿 초기화 작업 수행
    

---

### 2. `service(ServletRequest, ServletResponse)`

```java
@Override
public void service(ServletRequest arg0, ServletResponse arg1) throws ServletException, IOException {
	System.out.println("SERVICE() invoke...");
}
```

- 클라이언트의 모든 요청을 처리
    
- `doGet()` / `doPost()`가 아닌 **공통 진입점**
    
- 요청이 들어올 때마다 호출됨
    

---

### 3. `destroy()`

```java
@Override
public void destroy() {
	System.out.println("DESTROY() invoke...");
}
```

- 서블릿이 종료될 때 호출됨
    
- 메모리 해제, 연결 종료 등 정리 작업 수행
    

---

## ❗️주의사항

- 이 클래스에는 `@WebServlet` 어노테이션이 없음 → `web.xml`에 `<servlet>` / `<servlet-mapping>` 설정이 필요함
    

```xml
<servlet>
	<servlet-name>test2</servlet-name>
	<servlet-class>Servlet.C02Servlet_Test</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>test2</servlet-name>
	<url-pattern>/TEST_02</url-pattern>
</servlet-mapping>
```

---

## ✅ 핵심 정리

- `init()`, `service()`, `destroy()` 메서드 호출 순서를 실습하는 코드
    
- `@WebServlet`을 사용하지 않고 `web.xml` 매핑 방식을 사용할 때의 기본 구조
    
- 서블릿 실행 흐름이 동일하므로 `C01Servlet_Test`와 비교해서 이해하면 좋음
    

---
