
---

# 🧠 Spring Security 심화 구성

## ✅ 1. DB 기반 Remember-Me 설정

### 🔹 1-1. 테이블 생성

```sql
CREATE TABLE persistent_logins (
    username VARCHAR(64) NOT NULL,
    series VARCHAR(64) PRIMARY KEY,
    token VARCHAR(64) NOT NULL,
    last_used TIMESTAMP NOT NULL
);
```

### 🔹 1-2. SecurityConfig.java 설정

```java
@Bean
public PersistentTokenRepository persistentTokenRepository(DataSource dataSource) {
    JdbcTokenRepositoryImpl tokenRepository = new JdbcTokenRepositoryImpl();
    tokenRepository.setDataSource(dataSource);
    return tokenRepository;
}

@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .rememberMe()
            .key("remember-me-key")
            .tokenRepository(persistentTokenRepository(dataSource))
            .tokenValiditySeconds(86400) // 1일 유지
            .userDetailsService(userDetailsService()); // 인증 로직
}
```

---

## ✅ 2. CustomLoginSuccessHandler

### 🔹 2-1. Handler 클래스 생성

```java
@Component
public class CustomLoginSuccessHandler extends SimpleUrlAuthenticationSuccessHandler {

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request,
                                        HttpServletResponse response,
                                        Authentication authentication) throws IOException, ServletException {
        String username = authentication.getName();
        System.out.println("로그인 성공 사용자: " + username);
        super.onAuthenticationSuccess(request, response, authentication);
    }
}
```

### 🔹 2-2. SecurityConfig에 등록

```java
@Autowired
private CustomLoginSuccessHandler loginSuccessHandler;

@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .formLogin()
            .loginPage("/login")
            .successHandler(loginSuccessHandler);
}
```

---

## ✅ 3. CustomAccessDeniedHandler

### 🔹 3-1. Handler 클래스 생성

```java
@Component
public class CustomAccessDeniedHandler implements AccessDeniedHandler {

    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response,
                       AccessDeniedException accessDeniedException) throws IOException {
        response.sendRedirect("/accessDenied");
    }
}
```

### 🔹 3-2. SecurityConfig에 등록

```java
@Autowired
private CustomAccessDeniedHandler accessDeniedHandler;

@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .exceptionHandling()
            .accessDeniedHandler(accessDeniedHandler);
}
```

---

## ✅ 4. JSP에서 Spring Security 태그 사용

### 🔹 4-1. JSP 태그라이브러리 선언

```jsp
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>
```

### 🔹 4-2. 사용 예시

```jsp
<sec:authorize access="isAuthenticated()">
    <p>로그인한 사용자만 보이는 영역</p>
</sec:authorize>

<sec:authorize access="hasRole('ROLE_ADMIN')">
    <p>관리자 전용 메뉴</p>
</sec:authorize>
```

---

## 📌 요약

|항목|설명|
|---|---|
|DB 기반 Remember-Me|쿠키 + DB에 토큰 저장해 로그인 유지|
|커스텀 핸들러|로그인 성공 시 로직, 접근 거부 처리|
|Security 태그|JSP에서 권한별 조건 분기 출력 가능|

---
