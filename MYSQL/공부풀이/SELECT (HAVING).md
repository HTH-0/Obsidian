# 📘 SELECT (GROUP BY + HAVING)

---

### 📌 🔍 사용자별 총 구매금액이 1000 이상인 경우 조회

1. **🎯 쿼리 목적**:
    
    - 사용자별 총 구매금액이 1000 이상인 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, SUM(price * amount)
    FROM buytbl
    GROUP BY userid
    HAVING SUM(price * amount) >= 1000;
    ```
    
3. **🧠 세부 설명**:
    
    - `SELECT userid, SUM(price * amount)`: 사용자별로 총 구매금액 합산
        
    - `FROM buytbl`: 구매 정보 테이블
        
    - `GROUP BY userid`: 사용자별로 그룹화
        
    - `HAVING SUM(price * amount) >= 1000`: 그룹화된 결과 중 총합이 1000 이상인 경우만 필터링
        
4. **💡 추가 팁**:
    
    - `HAVING`은 `GROUP BY` 이후 조건 필터링 시 사용 (집계 함수 포함 가능)
        
    - `WHERE`은 집계 전에 사용
        

---

### 📌 🧍 지역별 평균 키가 175 이상인 경우 조회

1. **🎯 쿼리 목적**:
    
    - 지역별 평균 키가 175 이상인 지역 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, AVG(height) AS "평균키"
    FROM usertbl
    GROUP BY addr
    HAVING AVG(height) > 175;
    ```
    
3. **🧠 세부 설명**:
    
    - `SELECT addr, AVG(height)`: 지역별 평균 키 계산
        
    - `FROM usertbl`: 사용자 정보 테이블
        
    - `GROUP BY addr`: 지역별로 그룹화
        
    - `HAVING AVG(height) > 175`: 평균 키가 175 초과인 지역만 필터링
        
4. **💡 추가 팁**:
    
    - `AS "평균키"`처럼 따옴표를 쓰면 별칭에 한글 가능
        

---

### 📌 🛒 특정 사용자 조건(구매 3회 이상 & 총액 100 이상)

1. **🎯 쿼리 목적**:
    
    - 구매횟수가 3회 이상이고 총 구매액이 100 이상인 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, COUNT(*) AS 구매횟수, SUM(price * amount) 총구매액
    FROM buytbl
    GROUP BY userid
    HAVING COUNT(*) >= 3 AND SUM(price * amount) >= 100;
    ```
    
3. **🧠 세부 설명**:
    
    - `COUNT(*)`: 구매 건수 집계
        
    - `SUM(price * amount)`: 총 구매금액
        
    - `HAVING COUNT(*) >= 3 AND SUM(...) >= 100`: 두 조건 모두 만족하는 사용자 필터링
        
4. **💡 추가 팁**:
    
    - `HAVING` 절에서는 `AND`, `OR` 모두 사용 가능
        

---

### 📌 🗺️ 지역 + 상품그룹별 총 구매금액 집계

1. **🎯 쿼리 목적**:
    
    - 지역과 상품 그룹별 총 구매금액 합산 및 오름차순 정렬
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.addr, b.groupname, SUM(b.amount * b.price)
    FROM usertbl u
        JOIN buytbl b ON u.userid = b.userid
    GROUP BY u.addr, b.groupname
    ORDER BY SUM(b.amount * b.price);
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN`: 사용자와 구매 테이블 연결 (userid 기준)
        
    - `GROUP BY u.addr, b.groupname`: 지역 + 상품 그룹별 집계
        
    - `SUM(b.amount * b.price)`: 총 구매금액 계산
        
    - `ORDER BY`: 총 구매금액 기준 오름차순 정렬
        
4. **💡 추가 팁**:
    
    - `GROUP BY`에 2개 이상 컬럼 사용 가능 → 복합 그룹핑
        
    - `ORDER BY`에 집계 함수 사용 가능 (필드명이 아니라 전체 함수 사용 가능)