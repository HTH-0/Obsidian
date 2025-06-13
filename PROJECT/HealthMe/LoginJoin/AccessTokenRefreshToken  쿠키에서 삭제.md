---
aliases:
  - AccessToken/RefreshToken  쿠키에서 삭제
---
## JWT 로그아웃 처리 방식 (accessToken - 쿠키 방식)

### 목적

- 클라이언트와 서버 모두에서 accessToken 제거
    
- 로그아웃 시 인증 상태를 완전히 종료
    

---

### 전체 흐름

1. 클라이언트에서 `/logout` POST 요청 전송
    
2. 서버는 쿠키에 저장된 accessToken을 삭제
    
3. 클라이언트에서는 localStorage 정리
    
4. 최종적으로 메인 페이지로 이동
    

---

### 프론트엔드 (React)

```jsx
const handleLogout = async () => {
    try {
        await axios.post("/healthme/users/logout", {}, {
            withCredentials: true // 쿠키 포함
        });
    } catch (err) {
        console.warn("서버 로그아웃 실패", err);
    } finally {
        localStorage.removeItem("loginUser");
        window.location.href = "/";
    }
};
```

> accessToken은 HttpOnly 쿠키에 저장되어 있으므로 자바스크립트로 직접 접근 불가.  
> 따라서 localStorage에서 관리하던 사용자 정보만 정리하면 됨.

---


### 백엔드 (Spring Boot)

```java
@PostMapping("/logout")
public ResponseEntity<?> logout(HttpServletResponse response) {
    Cookie cookie = new Cookie("accessToken", null);
    cookie.setMaxAge(0);         // 즉시 만료
    cookie.setPath("/");         // 전체 경로에서 삭제
    cookie.setHttpOnly(true);    // 기존 설정과 동일하게
    response.addCookie(cookie);

    return ResponseEntity.ok("로그아웃 완료");
}
```

> HttpOnly 쿠키는 서버에서 명시적으로 삭제해야 함. JS에서 접근 불가.

---

### 핵심 요약

- accessToken은 HttpOnly 쿠키에 저장되어 보안상 안전함 (XSS 대응에 유리)
    
- 로그아웃 시 서버에서 `Max-Age=0`으로 쿠키 삭제
    
- 클라이언트는 localStorage만 초기화
    
- 별도의 상태관리 라이브러리 없이도 간단하게 로그아웃 처리 가능
    

---
