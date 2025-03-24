## 🔍 NULL 값 조건으로 데이터 조회

### 📌 NULL 데이터 조회하기

1. **🎯 쿼리 목적**:
    
    - 특정 컬럼(mobile1)의 값이 NULL인지 아닌지 조회
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl WHERE mobile1 IS NULL;
SELECT * FROM usertbl WHERE mobile1 IS NOT NULL;
```

3. **🧠 세부 설명**:
    
    - `SELECT *`: 테이블의 모든 컬럼 데이터를 조회
    - `FROM usertbl`: 데이터를 가져올 테이블 지정
    - `WHERE mobile1 IS NULL`: `mobile1` 컬럼 값이 비어있는(NULL인) 행만 조회
    - `WHERE mobile1 IS NOT NULL`: `mobile1` 컬럼 값이 비어있지 않은(NULL이 아닌) 행만 조회
4. **💡 추가 팁**:
    
    - `NULL`은 데이터가 없거나 아직 정해지지 않은 상태를 의미
    - `IS NULL`이나 `IS NOT NULL`을 사용할 때는 `=`를 쓰지 않음(예: `WHERE mobile1 = NULL`은 틀림)

---

## 🔎 중복 데이터 제거 후 조회

### 📌 중복 데이터 제외한 유일한 행만 조회하기

1. **🎯 쿼리 목적**:
    
    - 중복 데이터를 제거하고 고유한 행만 가져옴
2. **💻 SQL 구문**:
    

```sql
SELECT DISTINCT * FROM usertbl;
```

3. **🧠 세부 설명**:
    
    - `DISTINCT`: 중복된 행을 제거하고 고유(unique)한 행만 반환
    - 테이블 내의 모든 컬럼을 대상으로 중복 체크 수행
4. **💡 추가 팁**:
    
    - 중복 제거 시 속도에 영향을 줄 수 있으므로 꼭 필요한 경우에만 사용
    - 특정 컬럼만 중복 제거하려면 `SELECT DISTINCT 컬럼명 FROM 테이블명` 형태로 작성

---

## 📋 특정 컬럼 선택 및 컬럼 별칭 부여하여 조회

### 📌 원하는 컬럼 선택하고 별칭 지정하기

1. **🎯 쿼리 목적**:
    
    - 모든 컬럼이 아닌, 원하는 컬럼만 선택하여 조회하면서 컬럼명 변경하기
2. **💻 SQL 구문**:
    

```sql
SELECT name, addr, mobile1, mobile2 AS phone FROM usertbl;
```

3. **🧠 세부 설명**:
    
    - `SELECT name, addr, mobile1, mobile2`: 특정 컬럼만 선택하여 조회
    - `mobile2 AS phone`: `mobile2` 컬럼을 조회할 때 `phone`이라는 별칭(alias)으로 출력
4. **💡 추가 팁**:
    
    - 별칭(alias)은 컬럼명이 길거나 복잡할 때 간단히 나타내기 위해 사용
    - 별칭은 결과를 나타낼 때만 적용되며 실제 데이터베이스 컬럼명은 변경되지 않음