
---

# ✅ GRANT & REVOKE 핵심 정리

## 1. 📌 GRANT: 권한 부여

```sql
GRANT 권한종류 ON 객체 TO 사용자 [WITH GRANT OPTION];
```

|구성 요소|설명|
|---|---|
|`권한종류`|SELECT, INSERT, UPDATE, DELETE, EXECUTE 등|
|`객체`|테이블, 뷰, 시퀀스, 프로시저 등|
|`사용자`|특정 사용자 또는 `PUBLIC`|
|`WITH GRANT OPTION`|해당 사용자가 **다른 사용자에게도 권한 위임 가능**|

### 예시

```sql
GRANT SELECT ON EMP TO user1;
GRANT SELECT ON EMP TO user1 WITH GRANT OPTION;
GRANT INSERT, UPDATE ON DEPT TO user2;
GRANT SELECT ON EMP TO PUBLIC;
```

---

## 2. 📌 REVOKE: 권한 회수

```sql
REVOKE 권한종류 ON 객체 FROM 사용자;
```

|구성 요소|설명|
|---|---|
|`권한종류`|부여했던 권한을 지정|
|`객체`|테이블, 뷰 등 해당 권한의 대상|
|`사용자`|권한을 회수할 대상 사용자명|

### 예시

```sql
REVOKE SELECT ON EMP FROM user1;
REVOKE ALL ON EMP FROM PUBLIC;
```

---

## 3. 📌 WITH GRANT OPTION

|특징|설명|
|---|---|
|권한 위임 가능|받은 권한을 다른 사용자에게 부여할 수 있음|
|회수 전파|권한을 준 사용자의 권한이 회수되면, **그 권한을 위임받아 전달했던 사용자들까지 모두 회수됨**|

📌 예:

- A → B (WITH GRANT OPTION) → C 에게 권한 부여
    
- A가 B의 권한 REVOKE 시 → **C의 권한도 자동 회수**
    

---

## 4. 📌 PUBLIC

|항목|설명|
|---|---|
|의미|**데이터베이스의 모든 사용자**|
|용도|권한을 전체 사용자에게 한꺼번에 부여/회수|
|예시|`GRANT SELECT ON EMP TO PUBLIC;`|
|→ 모든 사용자에게 EMP 조회 가능||

---

## 5. 📌 권한 종류 (객체 권한 & 시스템 권한)

### 객체 권한

|권한|설명|
|---|---|
|SELECT|조회 권한|
|INSERT|삽입 권한|
|UPDATE|수정 권한|
|DELETE|삭제 권한|
|REFERENCES|외래키 참조 권한|
|EXECUTE|프로시저 실행 권한 (프로시저, 함수 등)|

### 시스템 권한 (관리자만 부여)

|권한|설명|
|---|---|
|CREATE TABLE|테이블 생성|
|CREATE USER|사용자 생성|
|DROP ANY TABLE|모든 테이블 삭제 권한 등|

---

## 6. 📌 권한 조회 (Oracle 기준)

```sql
-- 현재 사용자에게 부여된 객체 권한
SELECT * FROM USER_TAB_PRIVS;

-- 현재 사용자에게 부여된 시스템 권한
SELECT * FROM USER_SYS_PRIVS;

-- 현재 사용자에게 부여된 역할(Role)
SELECT * FROM USER_ROLE_PRIVS;
```

---

## 7. 📌 시험 포인트 요약

| 포인트                                              |     |
| ------------------------------------------------ | --- |
| WITH GRANT OPTION 없으면 → 권한 위임 ❌                  |     |
| REVOKE는 위임받은 권한도 전파적으로 회수됨                       |     |
| PUBLIC은 전체 사용자에게 권한 부여/회수                        |     |
| 권한은 객체(테이블, 뷰 등) 또는 시스템 수준으로 나뉨                  |     |
| GRANT/REVOKE는 **DCL 문장** (Data Control Language) |     |

---
