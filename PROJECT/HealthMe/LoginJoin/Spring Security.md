### Security 설정 – 경로별 권한 제어

#### 메서드 정의

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests((auth) -> {
        auth
            .requestMatchers("/", "/login", "/join").permitAll()
            .requestMatchers("/admin").hasRole("ADMIN")
            .requestMatchers("/my/**").hasAnyRole("ADMIN", "USER")
            .anyRequest().authenticated();
    });

    return http.build();
}
```

---

### 설정 해설

- `/`, `/login`, `/join`
    → 인증 없이 접근 허용 (`permitAll()`)
    
- `/admin`  
    → `ADMIN` 역할만 접근 가능 (`hasRole("ADMIN")`)
    
- `/my/**`  
    → `ADMIN`, `USER` 모두 접근 가능 (`hasAnyRole("ADMIN", "USER")`)
    
- 나머지 모든 요청  
    → 인증 필요 (`authenticated()`)
    

---

### 주의할 점

- `requestMatchers()`는 위에서부터 적용되므로 **더 좁은 경로를 먼저 배치**해야 함.
    
- `hasRole("ADMIN")` 내부적으로는 `ROLE_` prefix가 붙기 때문에, DB 또는 User 객체에 저장되는 권한은 `ROLE_ADMIN` 형태여야 함.
    
- 인증만 되면 접근 허용하려면 `authenticated()`  
    권한까지 따져야 하면 `hasRole()` 또는 `hasAnyRole()`
    

---
