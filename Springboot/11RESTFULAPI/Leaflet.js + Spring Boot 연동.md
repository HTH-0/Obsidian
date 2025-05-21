
---

# 🗺 Spring Boot RESTFUL API 실습 정리 - Part 4

## 🧭 Part 4: Leaflet.js + Spring Boot 연동

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|지도 라이브러리|Leaflet.js (오픈소스)|
|연동 방식|HTML + JS + Axios → Spring Boot REST API|
|활용 예시|마커 클릭 → 날씨 호출, 지도 클릭 → 팝업 표시, 행정경계 시각화 등|

---

## 📘 컨트롤러: `LeafletController.java`

```java
@Controller
@Slf4j
@RequestMapping("/leaflet")
public class LeafletController {

    @GetMapping("/index")
    public void index() {
        log.info("GET /leaflet/index...");
    }
}
```

- `GET /leaflet/index` 요청 시, `resources/templates/leaflet/index.html` 뷰 반환
    

---

## 🗂️ HTML 파일: `leaflet/index.html`

### ✅ 기본 지도 렌더링

```html
<div id="map" style="height:100vh; width:100%;"></div>

<script>
    var map = new L.Map('map', {
        center: new L.LatLng(35.829890, 128.532719), // 대구 중심
        zoom: 8,
        crs: L.Proj.CRS.Daum // Daum 지도 좌표계 적용
    });
    L.tileLayer.koreaProvider('DaumMap.Street').addTo(map);
</script>
```

- Daum Map 스타일 적용
    
- 중심 좌표 설정 (`35.829890, 128.532719`)
    

---

### ✅ 마커 + 팝업 + 툴팁

```javascript
const centerMaker = L.marker([35.829890, 128.532719]).addTo(map);
centerMaker.bindTooltip("중심 좌표").openTooltip();
centerMaker.bindPopup("<div>테스트 마커입니다</div>");
```

---

### ✅ 마커 클릭 → 날씨 API 호출 예시

```javascript
map.on('click', function(e) {
    const lat = e.latlng.lat;
    const lon = e.latlng.lng;

    const newMarker = L.marker([lat, lon]).addTo(map);

    axios.get(`/open/weather/get/${lat}/${lon}`)
        .then(resp => {
            const newContent = `
                <div>
                    <div>기준시간: ${resp.data.base}</div>
                    <div>구름량: ${resp.data.clouds.all}</div>
                </div>`;
            newMarker.bindPopup(newContent);
        })
        .catch(err => console.log(err));
});
```

- 지도 클릭 시 좌표값을 `/open/weather/get/{lat}/{lon}`에 요청
    
- 응답 결과를 마커 팝업으로 표시
    

---

### ✅ 행정 경계 GeoJSON 시각화

```javascript
axios.get("/geoJson/daegu.json")
    .then(resp => {
        const groupMap = {};
        const features = resp.data.features;

        features.forEach(f => {
            const sggnm = f.properties.sggnm;
            groupMap[sggnm] = groupMap[sggnm] || [];
            groupMap[sggnm].push(f);
        });

        const borderColors = {
            "중구": "red", "동구": "orange", "서구": "yellow",
            "남구": "green", "북구": "blue", "수성구": "navy",
            "달서구": "black", "달성군": "royalblue"
        };

        Object.entries(groupMap).forEach(([name, area]) => {
            L.geoJSON(area, {
                style: () => ({
                    color: borderColors[name],
                    fillColor: borderColors[name],
                    fillOpacity: 0.7,
                    weight: 3
                })
            }).addTo(map);
        });
    });
```

- GeoJSON 파일(`daegu.json`)을 이용해 구별 행정 경계를 구분하여 색상 렌더링
    

---

## 📌 정리 요약

|항목|설명|
|---|---|
|지도 라이브러리|Leaflet.js (오픈소스 + 간결함)|
|지도 출력 방식|HTML + JS + Tile Layer|
|이벤트 연동|마커 클릭, 지도 클릭, 마우스 오버 등|
|REST 연동|클릭 → `/open/weather/get/{lat}/{lon}` 요청 후 팝업|
|GeoJSON|구별로 행정경계 색상 시각화 가능|

---
