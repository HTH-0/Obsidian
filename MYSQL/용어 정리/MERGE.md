---

## ✅ MERGE 구문 정리

---

### 🔷 1. 정의

> `MERGE`는 **두 테이블 간의 데이터를 비교**하여, 조건에 따라 **INSERT**, **UPDATE**, **DELETE**를 실행하는 **다기능 통합 DML 문**입니다.

즉, `IF EXISTS THEN UPDATE ELSE INSERT` 같은 복잡한 조건을 **한 문장으로 처리**할 수 있음.

---

### 🔷 2. 기본 문법 구조

```sql
MERGE INTO 대상테이블 AS A
USING 기준테이블 AS B
ON (A.key = B.key)
WHEN MATCHED THEN
    UPDATE ...
    DELETE ...
WHEN NOT MATCHED THEN
    INSERT ...
```

|절|설명|
|---|---|
|`MERGE INTO`|값을 갱신하거나 추가할 **대상 테이블**|
|`USING`|비교 대상이 되는 **기준 테이블**|
|`ON`|비교 조건 (주로 기본키 또는 유니크 키)|
|`WHEN MATCHED`|ON 조건을 만족하는 경우 → `UPDATE` 또는 `DELETE` 수행|
|`WHEN NOT MATCHED`|ON 조건을 만족하지 않는 경우 → `INSERT` 수행|

---

### 🔷 3. WHEN MATCHED

- **기준 테이블(B)의 행이 대상 테이블(A)과 매칭되는 경우** 실행
    
- 보통 **UPDATE**나 **DELETE**에 사용
    

```sql
WHEN MATCHED THEN
  UPDATE SET A.col1 = B.col1
  WHERE A.col2 = '조건';

WHEN MATCHED THEN
  DELETE WHERE A.col3 < 1000;
```

---

### 🔷 4. WHEN NOT MATCHED

- 기준 테이블의 행이 대상 테이블에 **존재하지 않을 경우**
    
- 즉, 신규 데이터 → **INSERT**
    

```sql
WHEN NOT MATCHED THEN
  INSERT (A.id, A.name)
  VALUES (B.id, B.name)
  WHERE B.status = 'NEW';
```

---

### 🔷 5. 사용 예

#### 💡 신규 유저는 INSERT, 기존 유저는 UPDATE

```sql
MERGE INTO users u
USING new_data nd
ON (u.userid = nd.userid)
WHEN MATCHED THEN
  UPDATE SET u.email = nd.email
WHEN NOT MATCHED THEN
  INSERT (userid, email)
  VALUES (nd.userid, nd.email);
```

---

### 🔷 6. 장점

|항목|설명|
|---|---|
|통합 처리|INSERT, UPDATE, DELETE를 한 문장에 담을 수 있음|
|조건 분기|`WHEN MATCHED`, `WHEN NOT MATCHED`로 상세 제어 가능|
|가독성 ↑|복잡한 MERGE 로직을 간결하게 표현|

---

### 🔷 7. 주의사항

|항목|주의점|
|---|---|
|`ON` 조건|너무 모호하면 모든 행이 MATCH로 처리됨|
|`UPDATE`, `DELETE` 함께 쓸 때|`WHERE` 절 필수로 넣어주는 게 안전|
|`NOT MATCHED` 시 INSERT 대상 컬럼 주의|대상 테이블과 컬럼 순서/타입 맞춰야 함|

---

## ✅ 요약 암기

> **MERGE INTO 대상 USING 기준 ON 조건**  
> → **있으면(MATCHED) UPDATE/DELETE, 없으면(NOT MATCHED) INSERT**

---

필요하다면 이 내용을 기반으로 한 **MERGE 문제 예제, 실습 코드, 노션용 요약 카드**도 만들어드릴 수 있어요.