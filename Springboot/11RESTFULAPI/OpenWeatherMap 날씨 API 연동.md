
---

# 🌤 Spring Boot RESTFUL API 실습 정리 - Part 3

## 🌍 Part 3: OpenWeatherMap 날씨 API 연동

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|API 종류|OpenWeatherMap - 현재 날씨 조회|
|API 방식|RESTful GET|
|응답 포맷|JSON|
|처리 도구|`RestTemplate`, `UriComponentsBuilder`, Jackson|

---

## 📘 요청 방식

- **API 주소**: `https://api.openweathermap.org/data/2.5/weather`
    
- **요청 파라미터**:
    
    - `lat`: 위도
        
    - `lon`: 경도
        
    - `appid`: API Key
        

---

## 📦 컨트롤러: `OpenWeatherController.java`

```java
@RestController
@Slf4j
@RequestMapping("/open/weather")
public class OpenWeatherController {

    @GetMapping("/get/{lat}/{lon}")
    public ResponseEntity<Root> get(@PathVariable String lat, @PathVariable String lon) throws UnsupportedEncodingException {
        log.info("GET /open/weather/get...");

        String url = "https://api.openweathermap.org/data/2.5/weather";
        String apiKey = "your_api_key_here";

        URI uri = UriComponentsBuilder.fromHttpUrl(url)
            .queryParam("appid", URLEncoder.encode(apiKey, "UTF-8"))
            .queryParam("lat", lat)
            .queryParam("lon", lon)
            .build(true)
            .toUri();

        RestTemplate rt = new RestTemplate();
        ResponseEntity<Root> response = rt.exchange(uri, HttpMethod.GET, null, Root.class);

        return ResponseEntity.ok(response.getBody());
    }
}
```

---

## 📦 JSON 매핑 클래스 구조

```java
@Data
private static class Root {
    public Coord coord;
    public List<Weather> weather;
    public Main main;
    public Wind wind;
    public Clouds clouds;
    public Sys sys;
    public String name;
}

@Data
private static class Coord {
    public double lon;
    public double lat;
}

@Data
private static class Weather {
    public int id;
    public String main;
    public String description;
    public String icon;
}

@Data
private static class Main {
    public double temp;
    public double feels_like;
    public double temp_min;
    public double temp_max;
    public int pressure;
    public int humidity;
}

@Data
private static class Wind {
    public double speed;
    public int deg;
}

@Data
private static class Clouds {
    public int all;
}

@Data
private static class Sys {
    public String country;
    public int sunrise;
    public int sunset;
}
```

---

## 📌 정리 요약

|항목|설명|
|---|---|
|API 제공자|[OpenWeatherMap](https://openweathermap.org/current)|
|응답 방식|JSON|
|파라미터|위도/경도(lat/lon), appid|
|사용 기술|`RestTemplate`, `@PathVariable`, `Jackson`|
|활용 예|날씨 지도, 위치기반 날씨 알림, IoT 시스템 연동 등|

---
