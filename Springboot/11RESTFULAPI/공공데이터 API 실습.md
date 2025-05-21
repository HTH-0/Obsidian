
---

# 🌐 Spring Boot RESTFUL API 실습 정리 - Part 1

## ✅ 개요

- **프로젝트 이름**: `11RESTFUL_API`
    
- **기반 기술**: Spring Boot 3.x, RestTemplate, Jackson, JAXB, Thymeleaf
    
- **핵심 주제**: 외부 공공 API + 지도 API + SNS 인증/메시지 + 결제 API 실습
    

---

## 📁 실습 분류

|파트|주제|설명|
|---|---|---|
|Part 1|공공데이터 포털 (대구 돌발 교통, 기상청)|XML/JSON 기반 응답 수신 및 가공|
|Part 2|대구버스 실시간 위치 조회|JAXB 기반 XML 응답 처리|
|Part 3|OpenWeatherMap 날씨 조회|REST + Jackson 구조|
|Part 4|Leaflet.js 지도 연동|지도 출력, 마커 이벤트, GeoJSON|
|Part 5|Kakao 로그인 + 메시지|OAuth2 인증 및 메시지 전송|
|Part 6|Naver 로그인|OAuth2 인증 및 프로필 정보 조회|
|Part 7|Google Calendar API|일정 조회/등록 (FullCalendar 연동)|
|Part 8|PortOne 결제 연동|결제, 조회, 취소, 인증 API 연동|

---

## 🔹 Part 1: 공공데이터 API 실습

### 📦 의존성 (Gradle)

```groovy
// REST + JSON/XML 처리
implementation 'org.springframework.boot:spring-boot-starter-web'
implementation 'com.fasterxml.jackson.core:jackson-databind:2.19.0'
implementation 'com.fasterxml.jackson.dataformat:jackson-dataformat-xml:2.19.0'
implementation 'com.fasterxml.jackson.datatype:jackson-datatype-jsr310:2.19.0'
implementation 'com.fasterxml.jackson.core:jackson-annotations:2.19.0'
```

---

## 🚧 1-1. 대구 돌발 교통정보 API

### ✅ 컨트롤러: `OpenData01Controller`

```java
@RestController
@RequestMapping("/openData")
public class OpenData01Controller {
    @GetMapping("/unexpected")
    public void unexpected() {
        String url = "https://apis.data.go.kr/6270000/service/rest/dgincident";
        url += "?serviceKey=" + serviceKey + "&pageNo=1&numOfRows=10";

        RestTemplate rt = new RestTemplate();
        ResponseEntity<String> response = rt.exchange(url, HttpMethod.GET, null, String.class);
        System.out.println(response);
    }
}
```

- **요청 방식**: GET
    
- **응답 포맷**: XML (String으로 수신 → 추후 JAXB 매핑 가능)
    
- **주요 기능**: 돌발사고 목록 출력용 API 호출
    

---

## 🌦 1-2. 기상청 초단기 실황 조회

### ✅ 컨트롤러: `OpenData02Controller`

```java
@RestController
@RequestMapping("/openData")
public class OpenData02Controller {

    @GetMapping("/forcast")
    public void forcast() {
        String url = "http://apis.data.go.kr/1360000/.../getUltraSrtNcst?...";

        RestTemplate rt = new RestTemplate();
        ResponseEntity<Root> response = rt.exchange(url, HttpMethod.GET, null, Root.class);

        List<Item> list = response.getBody().getResponse().getBody().getItems().getItem();
        list.forEach(System.out::println);
    }

    @Data
    private static class Root { Response response; }
    @Data
    private static class Response { Body body; Header header; }
    @Data
    private static class Body { List<Item> item; }
    @Data
    private static class Item { String baseDate, baseTime, category, obsrValue; int nx, ny; }
}
```

- **요청 방식**: GET
    
- **응답 포맷**: JSON → Jackson `@Data` 클래스로 매핑
    
- **목표 기능**: 기상청 데이터 수집 → 리스트 출력
    

---
