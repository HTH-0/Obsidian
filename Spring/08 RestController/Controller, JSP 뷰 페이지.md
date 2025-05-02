
---

# ✅ RestTestController + JSP 뷰 페이지 (rest.jsp)

## 📦 RestTestController.java 전체 코드

```java
@Controller
@Slf4j
public class RestTestController {

    @Autowired
    private MemoService memoService;

    @GetMapping("/rest")
    public void rest() {
        log.info("GET /rest...");
        // /WEB-INF/views/rest.jsp 호출
    }

    @GetMapping("/add_get")
    public void add(MemoDto dto) {
        log.info("GET /add_get.." + dto);
        memoService.addMemo(dto);
        // 등록 후 별도 페이지 이동 없음
    }
}
```

---

## 🔍 Controller 설명

- `@Controller`: JSP 뷰를 반환하는 전통적인 MVC 방식 컨트롤러
    
- `/rest` 요청 → `/WEB-INF/views/rest.jsp` 페이지 진입
    
- `/add_get` 요청 → `MemoDto` 파라미터를 받아 DB 저장
    

---

## 🖥 rest.jsp 개요

### ✨ 페이지 목적

- 다양한 비동기 방식(XHR, AJAX, FETCH, AXIOS)을 테스트할 수 있도록 구성된 HTML UI
    

### 주요 블록 구조

|블록|설명|
|---|---|
|`.xhr-block`|XHR 방식으로 GET/POST/PUT/DELETE 테스트|
|`.ajax-block`|(미구현)|
|`.fetch-block`|(미구현)|
|`.axios-block`|Axios 방식으로 GET/POST/PUT/DELETE 테스트|
|`.result`|응답 결과 출력용 (비어 있음)|

---

## 🧪 Axios 요청 처리 예시

### ✅ Axios GET 요청

```javascript
axios.get(projectPath + "/memo/add_rest_get?id=1&text=hello")
  .then(resp => console.log(resp))
  .catch(err => console.log(err));
```

### ✅ Axios POST 요청

```javascript
const param = {
    "id": 1,
    "text": "hello"
};
const headers = { 'Content-Type': 'application/json' };

axios.post(projectPath + "/memo/add_rest_post", param, headers)
  .then(resp => console.log(resp))
  .catch(err => console.log(err));
```

### ✅ Axios PUT 요청

```javascript
axios.put(projectPath + "/memo/put/1/수정내용")
  .then(resp => console.log(resp))
  .catch(err => console.log(err));
```

### ✅ Axios DELETE 요청

```javascript
axios.delete(projectPath + "/memo/remove/1")
  .then(resp => console.log(resp))
  .catch(err => console.log(err));
```

---

## 📌 요약

- `RestTestController`는 `rest.jsp` 뷰를 연결해주는 역할
    
- `rest.jsp`는 Axios, XHR 등 다양한 비동기 방식으로 REST API를 호출하는 테스트용 UI
    
- JS에서 `projectPath`를 기준으로 `@RestController` API를 호출하는 구조
    
- 실제 API 백엔드는 `RestController_02Memo`가 담당
    

---
