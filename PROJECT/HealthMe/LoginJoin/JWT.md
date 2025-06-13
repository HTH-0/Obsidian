JWT 쿠키 기반 로그인 적용하면서 발생한 문제와 해결 과정 정리

---

1. accessToken / refreshToken 저장 위치 변경  
    기존에는 localStorage에 저장했는데, XSS에 취약하다는 점 때문에 쿠키(HttpOnly)로 변경함.  
    accessToken은 30분 refreshToken은 1달 정도가 일반적인 만료 설정임.
    
2. 로그인 후 응답이 안 오고 JSON parse 에러가 발생한 문제  
    React에서 로그인 요청을 보냈는데 빈 화면만 뜨고, 콘솔에는 JSON parse 에러가 났음.  
    axios 요청에 대해 백엔드가 정상적으로 응답하지 않았고, response.data가 undefined로 들어왔기 때문.
    
3. loginProcessingUrl을 설정하지 않은 문제  
    Spring Security에서 `.formLogin()` 사용할 때, loginProcessingUrl을 지정하지 않으면 기본값이 `/login`임.  
    프론트는 `/healthme/users/login`으로 요청을 보내고 있었는데, 백엔드는 `/login`으로 받으려고 해서 302 리다이렉트가 발생.  
    React의 axios는 이 리다이렉트된 응답을 처리하지 못하고 data가 비어 있는 상태로 넘어감.
    

→ 해결 방법

```java
.formLogin(login -> login
    .loginProcessingUrl("/healthme/users/login")
    ...
)
```

4. usernameParameter를 따로 지정하지 않아서 인증 실패가 발생함  
    프론트에서는 `userid`라는 키로 데이터를 보냈는데, 백엔드는 기본적으로 `username`이라는 파라미터를 찾음.  
    값이 매칭되지 않으니 인증 자체가 실패함.
    

→ 해결 방법

```java
.usernameParameter("userid")
```

5. axios 요청에 쿠키가 안 들어가는 문제  
    HttpOnly로 설정된 쿠키는 JS에서 접근할 수 없기 때문에, 클라이언트가 명시적으로 쿠키를 요청에 포함시켜야 함.  
    axios 설정에 `withCredentials: true`가 빠져 있으면 쿠키가 전송되지 않음.
    

→ 해결 방법

```js
axios.post('/healthme/users/login', data, {
  withCredentials: true
});
```

6. 백엔드 CORS 설정 부족  
    쿠키를 주고받으려면 CORS에서도 `credentials` 허용과 `Set-Cookie` 헤더 노출 설정이 필요함.  
    이게 없으면 프론트에 쿠키가 전달되지 않음.
    

→ 해결 방법

```java
config.setAllowedOrigins(List.of("http://localhost:3000"));
config.setAllowCredentials(true);
config.addExposedHeader("Set-Cookie");
```

7. 이후 발생한 구조분해 할당 문제  
    응답을 받지 못하고 `response.data === undefined` 상태에서  
    `const { accessToken, userInfo } = response.data;` 처럼 구조분해 하다 보니 에러 발생.  
    undefined가 localStorage에 저장되고, Header에서 `JSON.parse(undefined)` 시도하면서 또 에러.
    

---
