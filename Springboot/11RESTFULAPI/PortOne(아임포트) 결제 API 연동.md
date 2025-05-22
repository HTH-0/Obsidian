---
aliases:
  - PortOne(아임포트) 결제 API 연동**
---


---

# 💳 Spring Boot RESTFUL API 실습 정리 - Part 8

## 🧾 Part 8: PortOne 결제 & 본인인증 연동

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|서비스|[PortOne(아임포트)](https://portone.io/)|
|기능|결제 요청, 결제 조회, 결제 취소, 본인 인증|
|인증 방식|REST API 키와 시크릿을 통한 토큰 발급|
|요청 도구|`RestTemplate`, `HttpHeaders`, `HttpEntity`|

---

## 📘 1. 결제 요청 화면 (HTML)

```html
<button onClick="pay()">결제하기</button>
<button onClick="auth()">본인인증</button>

<script src="https://cdn.iamport.kr/v1/iamport.js"></script>
<script>
  IMP.init("imp_uid");

  function pay() {
    IMP.request_pay({
      channelKey: "channel-id",
      pay_method: "phone",
      merchant_uid: "merchant_" + crypto.randomUUID(),
      name: "테스트 결제",
      amount: 100,
      buyer_tel: "010-0000-0000"
    });
  }

  function auth() {
    IMP.certification({
      channelKey: "channel-id",
      merchant_uid: "test_" + new Date().toISOString()
    }, function(resp) {
      console.log(resp);
    });
  }
</script>
```

---

## 📦 2. 결제 토큰 발급

```java
@GetMapping("/getToken")
@ResponseBody
public void getToken() {
    String url = "https://api.iamport.kr/users/getToken";

    HttpHeaders headers = new HttpHeaders();
    MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
    params.add("imp_key", RESTAPI_KEY);
    params.add("imp_secret", RESTAPI_SECRET);

    HttpEntity<?> entity = new HttpEntity<>(params, headers);

    ResponseEntity<PortOneTokenResponse> response = new RestTemplate()
        .exchange(url, HttpMethod.POST, entity, PortOneTokenResponse.class);

    this.portOneTokenResponse = response.getBody();
}
```

---

## 📘 3. 결제 내역 조회

```java
@GetMapping("/getPayments")
@ResponseBody
public void getPayments() {
    String url = "https://api.iamport.kr/payments?imp_uid[]=imp_909948940398";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + portOneTokenResponse.getResponse().getAccess_token());
    header.add("Content-Type", "application/json");

    HttpEntity<?> entity = new HttpEntity<>(header);
    ResponseEntity<String> response = new RestTemplate()
        .exchange(url, HttpMethod.GET, entity, String.class);

    System.out.println(response);
}
```

---

## 📘 4. 결제 취소

```java
@GetMapping("/payment/cancel")
@ResponseBody
public void paymentCancel() {
    String url = "https://api.iamport.kr/payments/cancel";

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + portOneTokenResponse.getResponse().getAccess_token());
    header.add("Content-Type", "application/json");

    Map<String, Object> params = new HashMap<>();
    params.put("imp_uid", "");        // 취소할 결제 UID
    params.put("merchant_uid", "");   // 또는 주문번호

    HttpEntity<Map<String, Object>> entity = new HttpEntity<>(params, header);
    ResponseEntity<String> response = new RestTemplate()
        .exchange(url, HttpMethod.POST, entity, String.class);

    System.out.println(response.getBody());
}
```

---

## 📘 5. 본인인증 결과 조회

```java
@GetMapping("/certifications/{imp_uid}")
@ResponseBody
public void certifications(@PathVariable String imp_uid) {
    String url = "https://api.iamport.kr/certifications/" + imp_uid;

    HttpHeaders header = new HttpHeaders();
    header.add("Authorization", "Bearer " + portOneTokenResponse.getResponse().getAccess_token());
    header.add("Content-Type", "application/json");

    HttpEntity<?> entity = new HttpEntity<>(header);
    ResponseEntity<String> response = new RestTemplate()
        .exchange(url, HttpMethod.GET, entity, String.class);

    System.out.println(response);
}
```

---

## 📦 응답 DTO 구조 예시

```java
@Data
private static class PortOneTokenResponse {
    private int code;
    private Object message;
    private Response response;

    @Data
    private static class Response {
        private String access_token;
        private int now;
        private int expired_at;
    }
}
```

---

## ✅ 정리 요약

|항목|설명|
|---|---|
|초기 인증|`getToken`으로 access_token 발급|
|결제 요청|JS SDK (`IMP.request_pay`) 사용|
|결제 조회|`GET /payments?imp_uid=[]`|
|결제 취소|`POST /payments/cancel`|
|본인인증|`IMP.certification`, 결과는 `GET /certifications/{imp_uid}`|

---
