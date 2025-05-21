
---

# 🚌 Spring Boot RESTFUL API 실습 정리 - Part 2

## 🚍 Part 2: 대구버스 실시간 위치 조회 API

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|API 종류|공공데이터 포털 - 실시간 버스 위치 정보|
|응답 포맷|**XML**|
|파싱 방식|**JAXB** 사용 (`@XmlElement`, `@XmlRootElement`)|
|사용 클래스|`OpenData03Controller`, `BUSResult`, `ArrList` 등|

---

## 🧩 컨트롤러: `OpenData03Controller`

```java
@RestController
@Slf4j
@RequestMapping("/openData")
public class OpenData03Controller {

    @GetMapping("/bus/realtime")
    public void bus_realtime() throws UnsupportedEncodingException {
        String url = "https://apis.data.go.kr/6270000/dbmsapi01/getRealtime";
        String serviceKey = "..." // 실제 API 키
        String bsId = "7001001600";      // 정류장 ID
        String routeNo = "649";          // 노선 번호

        URI uri = UriComponentsBuilder.fromHttpUrl(url)
            .queryParam("serviceKey", URLEncoder.encode(serviceKey, "UTF-8"))
            .queryParam("bsId", bsId)
            .queryParam("routeNo", routeNo)
            .build(true)
            .toUri();

        RestTemplate rt = new RestTemplate();
        ResponseEntity<BUSResult> response = rt.exchange(uri, HttpMethod.GET, null, BUSResult.class);
        System.out.println(response.getBody());
    }
}
```

- **URI 구성**: `UriComponentsBuilder`를 활용하여 파라미터 인코딩 및 조립
    
- **RestTemplate**로 요청 → JAXB 방식으로 DTO 자동 매핑
    

---

## 📦 응답 매핑 클래스: `BUSResult.java`

```java
@XmlRootElement(name = "BUSResult")
@XmlAccessorType(XmlAccessType.FIELD)
@Data
public class BUSResult {
    private Header header;
    private Body body;

    @Data
    public static class Header {
        @XmlElement(name = "success")
        private boolean success;
        @XmlElement(name = "resultCode")
        private String resultCode;
        @XmlElement(name = "resultMsg")
        private String resultMsg;
    }

    @Data
    public static class Body {
        @XmlElement(name = "items")
        private Items items;
        @XmlElement(name = "totalCount")
        private int totalCount;
    }

    @Data
    public static class Items {
        private String routeNo;
        @XmlElementWrapper(name = "arrList")
        @XmlElement(name = "arrList")
        private List<ArrList> arrList;
    }

    @Data
    public static class ArrList {
        private String routeId;
        private String routeNo;
        private int moveDir;
        private int bsGap;
        private String bsNm;
        private String vhcNo2;
        private String busTCd2;
        private String busTCd3;
        private String busAreaCd;
        private String arrState;
        private int prevBsGap;
    }
}
```

---

## 📌 정리 요약

|항목|설명|
|---|---|
|요청 URI|대구버스 실시간 API|
|요청 도구|`RestTemplate` + `UriComponentsBuilder`|
|응답 구조|XML → JAXB 매핑 (`@XmlRootElement`, `@XmlElement`)|
|주요 처리|`arrList`를 통해 실시간 버스 도착 정보 획득|
|적용 기술|공공데이터 + XML 파싱 (JAXB)|

---
