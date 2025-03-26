# 📘 INNER JOIN (사용자 정보 + 구매정보 통합)

---

## 🔎 사용자와 구매 테이블 연결

### 📌 구매 내역이 있는 사용자 정보 조회

1. **🎯 쿼리 목적**:  
    구매 정보가 있는 사용자만 이름, 가격, 수량 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, b.price, b.amount
    FROM buytbl b
        FULL JOIN usertbl u
        ON u.userid = b.userid
    WHERE b.price IS NOT NULL;
    ```
    
3. **🧠 세부 설명**:
    
    - `FULL JOIN`: 양쪽 테이블 모두 포함
        
    - `WHERE b.price IS NOT NULL`: 구매정보 존재하는 행만 필터 → 사실상 `INNER JOIN`과 유사 효과
        
4. **💡 추가 팁**:
    
    - 더 명확하게는 `INNER JOIN` 사용 권장
        

---

### 📌 사용자별 총 구매금액 계산

1. **🎯 쿼리 목적**:  
    사용자별 구매금액(가격×수량)의 합계 구하고 내림차순 정렬
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, SUM(b.price * b.amount) "총 구매금액"
    FROM buytbl b
        JOIN usertbl u
        ON u.userid = b.userid
    GROUP BY u.name
    ORDER BY SUM(b.price * b.amount) DESC;
    ```
    
3. **🧠 세부 설명**:
    
    - `SUM(b.price * b.amount)`: 총 구매금액 계산
        
    - `GROUP BY u.name`: 사용자별 집계
        
    - `ORDER BY DESC`: 큰 금액부터 정렬
        

---

### 📌 '책'을 구매한 사용자 목록

1. **🎯 쿼리 목적**:  
    책을 구매한 사용자 이름만 중복 없이 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT DISTINCT(u.name)
    FROM usertbl u
        JOIN buytbl b
        ON b.userid = u.userid
    WHERE prodname = '책';
    ```
    
3. **🧠 세부 설명**:
    
    - `DISTINCT`: 중복 제거
        
    - `WHERE prodname = '책'`: 특정 상품 필터
        

---

### 📌 가입일이 2010년 이후인 사용자 + 구매 상품 조회

1. **🎯 쿼리 목적**:  
    최근 가입자들의 구매 내역 확인
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, u.mdate, b.prodname
    FROM usertbl u
        JOIN buytbl b
        ON u.userid = b.userid
    WHERE u.mdate > '2010-01-01';
    ```
    
3. **🧠 세부 설명**:
    
    - `u.mdate > '2010-01-01'`: 문자열 비교지만 날짜형처럼 동작 (Oracle에서는 가능)
        

---

### 📌 구매 건수가 가장 많은 사용자

1. **🎯 쿼리 목적**:  
    구매한 상품이 가장 많은 사용자 1명 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * FROM (
        SELECT u.name, COUNT(b.prodname)
        FROM usertbl u
            JOIN buytbl b
            ON u.userid = b.userid
        GROUP BY u.name
        ORDER BY COUNT(b.prodname) DESC
    )
    WHERE rownum = 1;
    ```
    
3. **🧠 세부 설명**:
    
    - `rownum = 1`: 최상위 1건 추출
        
    - `COUNT(b.prodname)`: 구매 건수 집계
        
4. **💡 추가 팁**:
    
    - Oracle 12c 이상에서는 `FETCH FIRST 1 ROWS ONLY` 도 가능
        

---

### 📌 키가 큰 사람의 구매상품 조회

1. **🎯 쿼리 목적**:  
    키가 175cm 이상인 사용자의 구매 상품 목록 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, b.prodname
    FROM usertbl u
        JOIN buytbl b
        ON u.userid = b.userid
    WHERE u.height > 175 AND b.prodname IS NOT NULL;
    ```
    
3. **🧠 세부 설명**:
    
    - `u.height > 175`: 조건 필터
        
    - `b.prodname IS NOT NULL`: 구매 상품 존재 필터
        

---

### 📌 분류별 총 구매 금액 (NULL 처리 포함)

1. **🎯 쿼리 목적**:  
    groupname 별 총 구매금액 집계, 분류 없으면 '미분류' 표시
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT NVL(b.groupname,'미분류') AS "b.groupname", SUM(b.price * b.amount)
    FROM buytbl b
    GROUP BY b.groupname;
    ```
    
3. **🧠 세부 설명**:
    
    - `NVL(column, '대체값')`: NULL을 대체하는 Oracle 함수
        
    - `GROUP BY`: 분류별 집계
        

---

### 📌 서울 거주자의 구매상품 조회

1. **🎯 쿼리 목적**:  
    주소가 서울인 사용자들의 구매상품, 수량 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, b.prodname, b.amount
    FROM usertbl u
        JOIN buytbl b
        ON u.userid = b.userid
    WHERE u.addr = '서울';
    ```
    

---

### 📌 상품별 구매한 사용자 수

1. **🎯 쿼리 목적**:  
    상품별로 몇 명의 사용자가 구매했는지 계산
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT b.prodname, COUNT(DISTINCT(u.name))
    FROM usertbl u
        JOIN buytbl b
        ON u.userid = b.userid
    GROUP BY b.prodname;
    ```
    
3. **🧠 세부 설명**:
    
    - `DISTINCT(u.name)`: 중복 사용자 제외
        

---

### 📌 사용자별 평균 구매금액

1. **🎯 쿼리 목적**:  
    사용자별 1건당 평균 구매금액 계산
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name, ROUND(SUM(price * amount)/COUNT(b.prodname))
    FROM usertbl u
        JOIN buytbl b
        ON u.userid = b.userid
    GROUP BY u.name;
    ```
    
3. **🧠 세부 설명**:
    
    - `ROUND`: 소수점 반올림
        
    - `SUM/COUNT`: 총액 ÷ 건수 → 평균 단가
        

---