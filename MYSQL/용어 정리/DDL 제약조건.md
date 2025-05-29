
---

## ✅ 제약조건(PK) 관련 DDL 문장 체크 기준표

| 항목                        | 체크 내용                                 | 설명                            | 예시                                                                |
| ------------------------- | ------------------------------------- | ----------------------------- | ----------------------------------------------------------------- |
| 1. **NOT NULL 여부**        | ✅ PK로 지정할 컬럼은 반드시 `NOT NULL`이어야 함     | `PRIMARY KEY`는 무조건 NULL 허용 불가 | `PROD_ID VARCHAR2(10) NOT NULL`                                   |
| 2. **`ADD` 키워드 위치**       | ✅ `ADD`는 `ALTER TABLE` 구문에서만 사용 가능    | `CREATE TABLE` 안에서 사용하면 문법 오류 | `ALTER TABLE PRODUCT ADD ...` (⭕)`CREATE TABLE (... ADD ...)` (❌) |
| 3. **`CONSTRAINT` 명시 여부** | ✅ 제약조건에 이름을 붙일 경우 `CONSTRAINT` 키워드 필수 | Oracle에서는 생략 불가               | `ADD CONSTRAINT 제약명 PRIMARY KEY (컬럼)`                             |

---


## ✅ 예시 비교

| 문장                                                         | 적절 여부 | 이유                             |
| ---------------------------------------------------------- | ----- | ------------------------------ |
| `ALTER TABLE ~ ADD CONSTRAINT PK_NAME PRIMARY KEY (COL)`   | ⭕     | ADD 사용 위치, CONSTRAINT 명시 모두 OK |
| `ALTER TABLE ~ ADD PRIMARY KEY PK_NAME (COL)`              | ❌     | CONSTRAINT 키워드 빠짐              |
| `CREATE TABLE (...) ADD CONSTRAINT PRIMARY KEY (COL)`      | ❌     | CREATE TABLE 안에서 ADD 사용 불가     |
| `CREATE TABLE (..., CONSTRAINT PK_NAME PRIMARY KEY (COL))` | ⭕     | 가장 적절한 선언 방식                   |

---

ALTER TABLE 에는 ADD CONSTRAINT 가능
CREATE TABLE 에는  ADD CONSTRAINT 불가능!