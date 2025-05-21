
---

# 💬 Spring Boot RESTFUL API 실습 정리 - Part 5

## 🟡 Part 5: Kakao 로그인 + 메시지 전송

---

## ✅ 핵심 기능 요약

|기능|설명|
|---|---|
|로그인|Kakao OAuth2 인증 → 인가코드 → 토큰 발급|
|나에게 메시지|Access Token을 이용해 `/v2/api/talk/memo/default/send` 호출|
|친구 목록|`/v1/api/talk/friends` API로 친구 정보 조회|
|친구에게 메시지|`/v1/api/talk/friends/message/default/send` 호출|

---

## 📘 1. 로그인 흐름

### ✅ 인가 코드 받기

```java
@GetMapping("/getCodeMsg")
public String getCode_message() {
    return "redirect:https://kauth.kakao.com/oauth/authorize?client_id=" + CLIENT_ID +
           "&redirect_uri=" + REDIRECT_URI +
           "&response_type=code&scope=talk_message";
}
```

- 인가 코드 수신 → 리다이렉션으로 유저가 인증
    

---

## 📘 2. 나에게 메시지 보내기

```java
@GetMapping("/message/me/{message}")
public void message_me(@PathVariable("message") String message) {
    String url = "https://kapi.kakao.com/v2/api/talk/memo/default/send";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + kakaoTokenResponse.getAccess_token());
    header.add("Content-Type", "application/x-www-form-urlencoded;charset=utf-8");

    JSONObject template = new JSONObject();
    template.put("object_type", "text");
    template.put("text", message);
    template.put("link", new JSONObject());
    template.put("button_title", "");

    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("template_object", template.toString());

    HttpEntity<MultiValueMap<String, String>> entity = new HttpEntity<>(params, header);
    ResponseEntity<String> response = new RestTemplate().exchange(url, HttpMethod.POST, entity, String.class);
}
```

---

## 📘 3. 친구 목록 조회

```java
@GetMapping("/friends")
public void getFriends() {
    String url = "https://kapi.kakao.com/v1/api/talk/friends";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + kakaoTokenResponse.getAccess_token());

    HttpEntity<?> entity = new HttpEntity<>(header);
    ResponseEntity<KakaoFriendsResponse> response = new RestTemplate()
        .exchange(url, HttpMethod.GET, entity, KakaoFriendsResponse.class);

    this.kakaoFriendsResponse = response.getBody();
}
```

### ✅ DTO 구조 예시

```java
@Data
private static class KakaoFriendsResponse {
    public List<Element> elements;
    public int total_count;
    public int favorite_count;
}

@Data
private static class Element {
    public String uuid;
    public String profile_nickname;
    public boolean allowed_msg;
}
```

---

## 📘 4. 친구에게 메시지 보내기

```java
@GetMapping("/message/friends/{message}")
public void friends_message(@PathVariable("message") String message) {
    String url = "https://kapi.kakao.com/v1/api/talk/friends/message/default/send";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + kakaoTokenResponse.getAccess_token());
    header.add("Content-Type", "application/x-www-form-urlencoded;charset=utf-8");

    JSONArray uuids = new JSONArray();
    for (Element friend : kakaoFriendsResponse.getElements()) {
        uuids.add(friend.getUuid());
    }

    JSONObject template = new JSONObject();
    template.put("object_type", "text");
    template.put("text", message);
    template.put("link", new JSONObject());
    template.put("button_title", "");

    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("template_object", template.toString());
    params.add("receiver_uuids", uuids.toString());

    HttpEntity<MultiValueMap<String, String>> entity = new HttpEntity<>(params, header);
    new RestTemplate().exchange(url, HttpMethod.POST, entity, String.class);
}
```

---

## ✅ 정리 요약

|항목|설명|
|---|---|
|인증 방식|OAuth2 인가코드 → AccessToken 획득|
|메시지 대상|나 / 친구|
|요청 헤더|`Authorization: Bearer {access_token}`|
|응답 처리|`RestTemplate` 사용|
|주의 사항|카카오 개발자 센터에서 **메시지 권한** 및 **로그인 Redirect URI 등록 필수**|

---

