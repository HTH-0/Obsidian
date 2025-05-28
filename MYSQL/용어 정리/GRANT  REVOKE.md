
---

## ✅ GRANT

> 특정 사용자에게 **권한을 부여**

### 📌 기본 문법

```sql
GRANT 권한종류 ON 객체 TO 사용자;
```

### 📌 예시

```sql
-- user1에게 EMP 테이블 조회 권한 부여
GRANT SELECT ON EMP TO user1;

-- user1에게 INSERT, UPDATE 권한도 같이 부여
GRANT INSERT, UPDATE ON EMP TO user1;
```

---

## ✅ REVOKE

> 특정 사용자로부터 **권한을 회수**

### 📌 기본 문법

```sql
REVOKE 권한종류 ON 객체 FROM 사용자;
```

### 📌 예시

```sql
-- user1의 EMP 테이블 SELECT 권한 회수
REVOKE SELECT ON EMP FROM user1;

-- 여러 권한 회수
REVOKE INSERT, UPDATE ON EMP FROM user1;
```

---

## ✅ 관련 개념들

|용어|설명|
|---|---|
|`SELECT`|테이블 조회 권한|
|`INSERT`|테이블에 행 추가 권한|
|`UPDATE`|테이블 수정 권한|
|`DELETE`|테이블에서 행 삭제 권한|
|`ALL`|위 권한 전체 부여/회수|
|`WITH GRANT OPTION`|부여받은 사용자가 **다른 사용자에게도 권한 위임 가능**|

---

### ✅ WITH GRANT OPTION 예시

```sql
GRANT SELECT ON EMP TO user1 WITH GRANT OPTION;
-- user1이 다른 사용자에게도 SELECT 권한 줄 수 있음
```

---

### ✅ 시스템 권한과 객체 권한의 차이

|구분|설명|예시|
|---|---|---|
|객체 권한|특정 테이블, 뷰, 시퀀스 등에 대한 권한|`SELECT ON EMP`, `INSERT ON DEPT`|
|시스템 권한|데이터베이스 전체에 영향을 주는 권한|`CREATE TABLE`, `CREATE USER`, `ALTER` 등|

---

## ✅ 정리 요약

|명령어|설명|예시|
|---|---|---|
|`GRANT`|사용자에게 권한 부여|`GRANT SELECT ON EMP TO user1;`|
|`REVOKE`|사용자 권한 회수|`REVOKE SELECT ON EMP FROM user1;`|
|`WITH GRANT OPTION`|권한 위임 가능|`GRANT SELECT ON EMP TO user1 WITH GRANT OPTION;`|

---
