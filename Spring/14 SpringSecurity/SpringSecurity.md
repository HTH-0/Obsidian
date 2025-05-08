

---

# 🛡️ Spring Security 실습 정리 (STS3 + Spring 5.0.7.RELEASE)

## ✅ 1. Spring Security 정의

- 인증(Authentication)과 인가(Authorization)을 제공하는 스프링 기반 보안 프레임워크
    
- 필터 체인 기반 구조로 웹 애플리케이션, API 보안 설정 가능
    
- 주요 지원 기능:
    
    - 로그인/로그아웃, 세션 관리, Remember-Me
        
    - URL/메서드 단위 권한 제어
        
    - CSRF, CORS 보안
        
    - 비밀번호 암호화 및 사용자 인증 커스터마이징
        

---

## ✅ 2. Maven 의존성 설정 (pom.xml)

```xml
<!-- Spring Security Web -->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-web</artifactId>
    <version>5.0.7.RELEASE</version>
</dependency>

<!-- Spring Security Config -->
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
    <version>5.0.7.RELEASE</version>
</dependency>
```

---

## ✅ 3. SecurityFilterChain 개요

> Spring Security는 다수의 Filter로 구성된 **Filter Chain 구조**로 작동  
> 인증/인가/세션/로그아웃/Remember-Me 등 모든 기능이 이 체인 내에서 작동

(참고이미지)

---

## ✅ 4. 기본 보안 설정 (SecurityConfig.java)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .anyRequest().authenticated()
                .and()
            .formLogin(); // 기본 로그인 페이지 사용
    }
}
```

- `@EnableWebSecurity`: Spring Security 활성화
    
- `formLogin()`: 기본 제공 로그인 폼 사용
    
- 모든 요청은 인증 필요
    

---

## ✅ 5. 사용자 인증

### 🔹 In-Memory 방식

```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth.inMemoryAuthentication()
        .withUser("admin").password("{noop}1234").roles("ADMIN")
        .and()
        .withUser("user").password("{noop}1234").roles("USER");
}
```

- `{noop}`: 비밀번호 인코딩 생략 (테스트 전용)
    

### 🔹 JDBC 인증 (DB 연동)

```java
@Autowired
private DataSource dataSource;

@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth.jdbcAuthentication()
        .dataSource(dataSource)
        .usersByUsernameQuery("SELECT username, password, enabled FROM users WHERE username = ?")
        .authoritiesByUsernameQuery("SELECT username, authority FROM authorities WHERE username = ?");
}
```

- 사용자 테이블(users), 권한 테이블(authorities) 필요
    

---

## ✅ 6. 인가 (Authorization) 설정

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .antMatchers("/admin/**").hasRole("ADMIN")
            .antMatchers("/user/**").hasAnyRole("ADMIN", "USER")
            .anyRequest().authenticated()
            .and()
        .formLogin();
}
```

- URL 패턴별로 접근 권한 제한 가능
    

---

## ✅ 7. 커스텀 로그인/로그아웃 설정

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .formLogin()
            .loginPage("/login")
            .defaultSuccessUrl("/home")
            .failureUrl("/login?error=true")
            .permitAll()
        .and()
        .logout()
            .logoutUrl("/logout")
            .logoutSuccessUrl("/login?logout=true")
            .invalidateHttpSession(true)
            .deleteCookies("JSESSIONID");
}
```

- 커스텀 로그인/로그아웃 경로 설정 가능
    

---

## ✅ 8. 예외 처리

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .exceptionHandling()
            .accessDeniedPage("/accessDenied");
}
```

- 접근 거부 시 이동할 뷰 경로 지정 필요 (`accessDenied.jsp` 등)
    

---

## ✅ 9. 세션/Remember-Me 기능

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .sessionManagement()
            .maximumSessions(1) // 세션 1개 제한
            .maxSessionsPreventsLogin(false); // 기존 세션 무효화 후 새 로그인 허용
}
```

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
    http
        .rememberMe()
            .key("remember-me-key")
            .tokenValiditySeconds(86400) // 1일
            .userDetailsService(userDetailsService()); // 사용자 로딩 전략 지정
}
```

- `rememberMe()`를 통해 **로그인 유지(자동 로그인)** 기능 제공
    

---

## 📌 요약

|항목|설명|
|---|---|
|인증 설정|InMemory, JDBC, Custom UserDetailsService|
|인가 설정|URL 패턴 또는 메서드(@Secured, @PreAuthorize) 기반|
|로그인/로그아웃|커스텀 페이지 지정 및 성공/실패 처리 가능|
|예외 처리|접근 거부 시 페이지 이동 설정 가능|
|세션 관리|동시 로그인 제한 가능 (`maximumSessions`)|
|Remember-Me|쿠키 기반 자동 로그인 기능|

---
