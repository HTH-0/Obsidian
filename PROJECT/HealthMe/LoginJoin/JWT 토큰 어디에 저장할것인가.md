---
aliases:
  - JWT 토큰 어디에 저장할것인가?
---
## 로그인 상태 유지 관련 트러블슈팅 기록

### 문제 상황

- 로그인 시 access token을 **localStorage에 저장**하는 방식으로 구현되어 있었음.
    
- `userInfo`, `accessToken` 등이 localStorage에 함께 저장되어 있었고,  
    로그인 상태 여부도 이 데이터를 기준으로 판단함.
    
- 로그아웃 시 localStorage에서 해당 항목을 제거했지만,  
    간헐적으로 인증 상태가 유지되는 문제가 발생함.
    

### 원인 분석

- `accessToken`을 localStorage에 저장하면 **프론트엔드에서 접근이 용이**하다는 장점이 있으나,  
    브라우저의 JavaScript에서 직접 접근 가능하기 때문에 **XSS 공격에 매우 취약**함.
    
- 즉, 악의적인 스크립트가 삽입되면 사용자의 토큰을 탈취할 수 있음.
    
- 실제 서비스 환경에서는 **보안 리스크가 크기 때문에 부적절한 방식**임.
    

---

### 해결 방법

- accessToken과 refreshToken의 저장 위치를 **HTTP Only 쿠키로 변경**함.
    
- HTTP Only 속성을 통해 JavaScript로 접근할 수 없게 함으로써 XSS 위험 제거.
    
- 로그아웃 시 서버에 요청을 보내 쿠키 만료 처리도 함께 수행하도록 구조 변경.
    

#### 변경 전 (문제 방식)

```js
// 로그인 시
localStorage.setItem("accessToken", token);

// API 요청 시
const token = localStorage.getItem("accessToken");
axios.defaults.headers.common["Authorization"] = `Bearer ${token}`;
```

#### 변경 후 (보안 강화 방식)

- accessToken을 서버에서 `HttpOnly` 쿠키로 설정
    
- 클라이언트에서는 토큰을 직접 다루지 않고, 쿠키 기반 인증 사용
    

```java
// Spring에서 토큰을 쿠키로 전송
ResponseCookie accessTokenCookie = ResponseCookie.from("accessToken", token)
        .httpOnly(true)
        .secure(true)
        .path("/")
        .maxAge(60 * 30)
        .build();
response.addHeader("Set-Cookie", accessTokenCookie.toString());
```

---

### 결과

- 프론트에서는 더 이상 `accessToken`을 localStorage에 저장하지 않음
    
- 인증 관련 로직은 서버가 관리하는 쿠키 기반으로 전환됨
    
- XSS로 인한 토큰 탈취 가능성이 제거됨
    
- 로그아웃 시 서버와 연동하여 토큰 쿠키를 삭제 → 완전한 인증 종료 가능
    

---

### 정리

- **localStorage에 accessToken을 저장하는 방식은 구현이 간편하지만, 보안상 매우 위험**
    
- **HTTP Only 쿠키는 XSS 공격으로부터 안전한 저장 방식이며, 실서비스에 적합**
    
- 인증 토큰은 가능한 한 **프론트엔드에서 직접 다루지 않는 방식**이 권장됨
    
- 인증 상태 판별, 로그아웃 처리 등도 모두 쿠키 중심으로 재구성 필요
    
