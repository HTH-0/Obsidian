

---

# ✅ Spring Boot RESTFUL API 실습 정리 - Part 6

## 🟩 Part 6: Naver 로그인 & 프로필 조회

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|인증 방식|OAuth2 Authorization Code Grant|
|API 제공자|네이버 개발자 센터|
|인증 흐름|인가 코드 발급 → Access Token 발급 → 사용자 프로필 조회|
|처리 기술|`RestTemplate`, `HttpEntity`, `@RequestParam`, `RedirectView`|

---

## 📘 1. 로그인 요청 (인가 코드 받기)

```java
@GetMapping("/login")
public String login() {
    return "redirect:https://nid.naver.com/oauth2.0/authorize?response_type=code" +
           "&client_id=" + NAVER_CLIENT_ID +
           "&state=STATE_STRING" +
           "&redirect_uri=" + REDIRECT_URL;
}
```

- 브라우저에서 로그인 → Redirect URI로 `code`, `state` 반환됨
    

---

## 📘 2. Callback 처리 및 토큰 발급

```java
@GetMapping("/callback")
public String callback(@RequestParam("code") String code, @RequestParam("state") String state) {
    String url = "https://nid.naver.com/oauth2.0/token";

    HttpHeaders header = new HttpHeaders();

    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("grant_type", "authorization_code");
    params.add("client_id", NAVER_CLIENT_ID);
    params.add("client_secret", NAVER_CLIENT_SECRET);
    params.add("code", code);
    params.add("state", state);

    HttpEntity<MultiValueMap<String, String>> entity = new HttpEntity<>(params, header);

    ResponseEntity<NaverTokenResponse> response = new RestTemplate()
        .exchange(url, HttpMethod.POST, entity, NaverTokenResponse.class);

    this.naverTokenResponse = response.getBody();
    return "redirect:/naver/main";
}
```

---

## 📘 3. 사용자 프로필 조회

```java
@GetMapping("/main")
public void main(Model model) {
    String url = "https://openapi.naver.com/v1/nid/me";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + this.naverTokenResponse.getAccess_token());

    HttpEntity<?> entity = new HttpEntity<>(header);

    ResponseEntity<NaverProfileResponse> response = new RestTemplate()
        .exchange(url, HttpMethod.POST, entity, NaverProfileResponse.class);

    model.addAttribute("profile", response.getBody());
}
```

---

## 📦 응답 매핑 DTO

```java
@Data
private static class NaverTokenResponse {
    private String access_token;
    private String refresh_token;
    private String token_type;
    private String expires_in;
}

@Data
private static class NaverProfileResponse {
    private String resultcode;
    private String message;
    private Response response;
}

@Data
private static class Response {
    private String id;
    private String email;
    private String name;
    private String profile_image;
}
```

---

## 📘 4. 연결 해제 및 로그아웃

### ✅ 로그아웃

```java
@GetMapping("/logout")
public String disConnect() {
    return "redirect:https://nid.naver.com/nidlogin.logout?returl=https://www.naver.com/";
}
```

### ✅ 연결 해제 (unlink)

```java
@GetMapping("/unlink")
public void logout() {
    String url = "https://nid.naver.com/oauth2.0/token";

    HttpHeaders header = new HttpHeaders();

    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("grant_type", "delete");
    params.add("client_id", NAVER_CLIENT_ID);
    params.add("client_secret", NAVER_CLIENT_SECRET);
    params.add("access_token", this.naverTokenResponse.getAccess_token());

    HttpEntity<MultiValueMap<String, String>> entity = new HttpEntity<>(params, header);

    ResponseEntity<String> response = new RestTemplate()
        .exchange(url, HttpMethod.POST, entity, String.class);

    System.out.println(response.getBody());
}
```

---

## ✅ 정리 요약

|항목|설명|
|---|---|
|인가 코드|Redirect URL로 `code`, `state` 전달받음|
|토큰 요청|POST `/oauth2.0/token`|
|사용자 정보|POST `/v1/nid/me`|
|로그아웃|URL 리다이렉션 방식|
|연결 해제|토큰 삭제 방식 (`grant_type=delete`)|

---


