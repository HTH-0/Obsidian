# 📘 NULL 처리 함수 정리 (NVL, NVL2, NULLIF, COALESCE)

---

## 🧪 NULL 처리 함수

---

### 📌 🧯 NVL 함수

1. **🎯 쿼리 목적**:
    
    - NULL 값을 기본값으로 대체
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, mobile1, mobile2,
           NVL(mobile1, '없음') AS nvl_mobile1,
           NVL(height, 0) AS nvl_height
    FROM userTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `NVL(컬럼, 대체값)`  
        → 컬럼이 `NULL`이면 지정한 대체값 반환
        

---

### 📌 🔁 NVL2 함수

1. **🎯 쿼리 목적**:
    
    - NULL 여부에 따라 서로 다른 값을 반환
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, mobile1,
           NVL2(mobile1, '연락처 있음', '연락처 없음') AS contact_status,
           NVL2(mobile1, mobile1 || '-' || mobile2, '연락처 없음') AS full_mobile
    FROM userTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `NVL2(컬럼, not_null_값, null_값)`
        
    - 컬럼이 `NULL`이 아니면 첫 번째 값, `NULL`이면 두 번째 값 반환
        

---

### 📌 ❔ NULLIF 함수

1. **🎯 쿼리 목적**:
    
    - 두 값이 같으면 `NULL` 반환, 다르면 첫 번째 값 유지
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT prodName, price, amount,
           NULLIF(price, 30) AS nullif_price
    FROM buyTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `NULLIF(A, B)`: A = B 이면 NULL, 아니면 A 반환
        
    - 조건 분기 없이 비교 결과로 NULL 처리할 때 사용
        

---

### 📌 🪣 COALESCE 함수

1. **🎯 쿼리 목적**:
    
    - 여러 값 중 NULL이 아닌 첫 번째 값을 반환
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, 
           COALESCE(mobile1, mobile2, '연락처 없음') AS contact,
           COALESCE(NULL, NULL, '모두 NULL') AS test_coalesce
    FROM userTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `COALESCE(값1, 값2, ..., 기본값)`
        
    - 왼쪽부터 차례로 NULL이 아닌 값을 반환 (NVL 다중 버전)
        
4. **💡 추가 팁**:
    
    - `NVL`은 2개 값만 처리 가능, `COALESCE`는 다중 처리에 유리
        

---