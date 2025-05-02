
---

# ✅ `RestController_02Memo` – 메모 CRUD REST API

## 📦 전체 코드

```java
@RestController
@Slf4j
@RequestMapping("/memo")
public class RestController_02Memo {

    @Autowired
    private MemoService memoService;

    // 전체 메모 확인
    @GetMapping(value="/getAll", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    public List<MemoDto> getAll() {
        log.info("GET /memo/getAll");
        return memoService.getAllMemo();
    }

    // 단건 조회 (id만 로그)
    @GetMapping("/get/{id}")
    public void get(@PathVariable int id) {
        log.info("GET /memo/get... " + id);
    }

    // 메모 등록 - GET 방식 (쿼리 파라미터 이용)
    @GetMapping("/add_rest_get")
    public void add_get(MemoDto dto) {
        log.info("GET /memo/add_rest_get.." + dto);
        memoService.addMemo(dto);
    }

    // 메모 등록 - POST 방식 (JSON body 이용)
    @PostMapping("/add_rest_post")
    public void add(@RequestBody MemoDto dto) {
        log.info("POST /memo/add_rest_post.." + dto);
        memoService.addMemo(dto);
    }

    // 메모 수정 - PUT 방식 (PathVariable 사용)
    @PutMapping("/put/{id}/{text}")
    public void put(MemoDto dto) {
        log.info("PUT /memo/put.." + dto);
        memoService.modifyMemo(dto);
    }

    // 메모 수정 - PUT 방식 (RequestBody JSON 이용)
    @PutMapping("/put2")
    public void put2(@RequestBody MemoDto dto) {
        log.info("PUT /memo/put2.." + dto);
        memoService.modifyMemo(dto);
    }

    // 메모 수정 - PATCH 방식 (미구현)
    @PatchMapping("/patch/{id}/{text}")
    public void patch(MemoDto dto) {
        log.info("PATCH /memo/patch.." + dto);
        // 실제 서비스 로직 없음
    }

    // 메모 삭제
    @DeleteMapping("/remove/{id}")
    public void remove(@PathVariable int id) {
        log.info("DELETE /memo/remove.." + id);
        memoService.removeMemo(id);
    }
}
```

---

## 🔍 코드 분석

### ✅ 컨트롤러 개요

- `@RestController` + `@RequestMapping("/memo")`: `/memo` 경로로 들어오는 요청을 처리
    
- `MemoService`를 주입 받아 실제 DB 작업 수행
    

---

### ✅ CRUD API 설명

|메서드|설명|URL 예시|
|---|---|---|
|`getAll()`|전체 메모 조회|`GET /memo/getAll`|
|`get(id)`|단건 조회(현재는 로그만 출력)|`GET /memo/get/1`|
|`add_get(dto)`|GET 방식 등록 (쿼리 파라미터)|`GET /memo/add_rest_get?id=1&text=abc`|
|`add(dto)`|POST 방식 등록 (JSON)|`POST /memo/add_rest_post`|
|`put(dto)`|Path 방식 수정|`PUT /memo/put/1/수정내용`|
|`put2(dto)`|JSON 기반 수정|`PUT /memo/put2`|
|`patch(dto)`|PATCH 방식 (미구현)|`PATCH /memo/patch/1/부분수정`|
|`remove(id)`|메모 삭제|`DELETE /memo/remove/1`|

---

## 💡 비동기 요청과의 연결 포인트

- `add_rest_post`나 `put2`, `remove` 등의 메서드는 axios 또는 fetch 등 비동기 요청에서 자주 호출됨
    
- 특히 `@RequestBody`를 사용하는 POST/PUT 메서드는 JSON 구조의 요청 본문을 받을 수 있음
    

---

## 📌 요약

- `RestController_02Memo`는 RESTful 설계에 따라 CRUD를 API로 구현한 실제 예제
    
- 다양한 요청 방식(GET/POST/PUT/DELETE)을 테스트할 수 있는 구조로 설계
    
- 프론트엔드 비동기 요청과 직접 연동되는 백엔드 핵심 컨트롤러
    

---
