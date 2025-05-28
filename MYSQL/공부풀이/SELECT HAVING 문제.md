# 📘 GROUP BY + HAVING 문제 정리

---

### 📌 ① 사용자별 총 구매금액이 1000 이상인 경우

1. **🎯 쿼리 목적**:
    
    - 사용자별로 총 구매금액이 1000 이상인 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, SUM(price * amount)
    FROM buytbl
    GROUP BY userid
    HAVING SUM(price * amount) >= 1000;
    ```
    
3. **🧠 세부 설명**:
    
    - `GROUP BY userid`: 사용자별로 그룹화
        
    - `SUM(price * amount)`: 총 구매금액 계산
        
    - `HAVING`: 집계 조건 필터링
        

---

### 📌 ② 지역별 사용자 수가 2명 이상인 경우

1. **🎯 쿼리 목적**:
    
    - 지역별 사용자 수가 2명 이상인 지역 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, COUNT(userid)
    FROM usertbl
    GROUP BY addr
    HAVING COUNT(userid) >= 2;
    ```
    
3. **🧠 세부 설명**:
    
    - `GROUP BY addr`: 지역별 그룹화
        
    - `COUNT(userid)`: 사용자 수 계산
        
    - `HAVING`: 사용자 수가 2 이상인 경우만 출력
        

---

### 📌 ③ 상품별 평균 가격이 100 이상인 경우

1. **🎯 쿼리 목적**:
    
    - 상품명 기준 평균 가격이 100 이상인 상품 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT prodName, AVG(price)
    FROM buytbl
    GROUP BY prodname
    HAVING AVG(price) >= 100;
    ```
    
3. **🧠 세부 설명**:
    
    - `GROUP BY prodname`: 상품명 기준 그룹화
        
    - `AVG(price)`: 평균 가격 계산
        

---

### 📌 ④ 출생년도별 평균 키가 175 이상

1. **🎯 쿼리 목적**:
    
    - 출생년도별 평균 키가 175 이상인 데이터 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT birthyear, AVG(height)
    FROM usertbl
    GROUP BY birthyear
    HAVING AVG(height) >= 175;
    ```
    

---

### 📌 ⑤ 사용자별로 2개 이상의 상품 구매한 경우

1. **🎯 쿼리 목적**:
    
    - 상품 구매 수가 2건 이상인 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, COUNT(prodname)
    FROM buytbl
    GROUP BY userid
    HAVING COUNT(prodname) >= 2;
    ```
    

---

### 📌 ⑥ 지역별 총 구매금액이 200 초과

1. **🎯 쿼리 목적**:
    
    - 사용자와 구매 테이블을 조인하여 지역별 총 구매금액이 200 초과인 지역 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, SUM(price)
    FROM buytbl b
        JOIN usertbl u ON u.userid = b.userid
    GROUP BY addr
    HAVING SUM(price) > 200;
    ```
    

---

### 📌 ⑦ 구매 횟수 3회 이상 & 총 구매금액 500 이상인 사용자

1. **🎯 쿼리 목적**:
    
    - 조건을 모두 만족하는 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, COUNT(prodname), SUM(price * amount)
    FROM buytbl
    GROUP BY userid
    HAVING COUNT(prodname) >= 3 AND SUM(price * amount) >= 500;
    ```
    

---

### 📌 ⑧ 평균 키가 가장 높은 지역 조회 (서브쿼리 포함)

1. **🎯 쿼리 목적**:
    
    - 지역별 평균 키 중 가장 높은 지역만 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, AVG(height)
    FROM usertbl
    GROUP BY addr
    HAVING AVG(height) = (
        SELECT MAX(avg) FROM (
            SELECT addr, AVG(height) avg
            FROM usertbl
            GROUP BY addr
        )
    );
    ```
    

---

### 📌 ⑨ 사용자별 총 구매금액이 전체 평균보다 큰 경우

1. **🎯 쿼리 목적**:
    
    - 전체 평균보다 더 많이 구매한 사용자 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, SUM(price * amount)
    FROM buytbl
    GROUP BY userid
    HAVING SUM(price * amount) > (
        SELECT AVG(price * amount)
        FROM buytbl
    );
    ```
    

---

### 📌 ⑩ 각 지역별 사용자별 총 구매금액이 지역 평균보다 높은 경우

1. **🎯 쿼리 목적**:
    
    - 사용자별 총 구매금액이 해당 지역 평균 이상인 경우 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT b.userid, u.addr, SUM(price * amount)
    FROM buytbl b
        JOIN usertbl u ON b.userid = u.userid
    GROUP BY u.addr, b.userid
    HAVING SUM(b.price * b.amount) > (
        SELECT AVG(sumVal)
        FROM (
            SELECT u2.addr, SUM(b2.price * b2.amount) AS sumVal 
            FROM buytbl b2
                JOIN usertbl u2 ON u2.userid = b2.userid
            GROUP BY u2.addr
        )
    );
    ```
    
3. **💡 추가 팁**:
    
    - 복잡한 조건 필터링은 **서브쿼리**를 사용하여 해결
        
    - `GROUP BY` 다중 컬럼(지역 + 사용자 ID) 활용
        

---