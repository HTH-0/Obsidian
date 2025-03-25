# 📘 Oracle SQL 함수 정리 (단일행, 다중행, 분석 함수)

---

## 🔎 조건에 따라 값을 다르게 출력하는 함수

### 📌 DECODE 함수 (Oracle 전용)

1. **🎯 쿼리 목적**:
    
    - 주소 값에 따라 지역명을 조건부로 변환
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, addr,
           DECODE(addr, 
                  '서울', '수도권', 
                  '경기', '수도권',
                  '경남', '경상권',
                  '경북', '경상권',
                  '전남', '전라권',
                  '기타') AS region
    FROM userTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `DECODE(컬럼, 조건1, 결과1, 조건2, 결과2, ..., 기본값)` 형식
        
    - Oracle 전용 함수로 간단한 조건 분기 처리에 사용
        
    - `CASE`보다 간단하지만 복잡한 로직에는 부적합
        

---

### 📌 CASE 표현식 (표준 SQL)

#### 🔸 단순 CASE

1. **🎯 쿼리 목적**:
    
    - 상품 그룹명에 따라 카테고리명을 지정
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userID, prodName,
           CASE groupName
               WHEN '전자' THEN '디지털'
               WHEN '의류' THEN '패션'
               WHEN '서적' THEN '교육'
               ELSE '기타'
           END AS category
    FROM buyTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `CASE column WHEN value THEN result ... ELSE default END`
        
    - 단일 컬럼 기준 조건 분기
        

---

#### 🔸 검색 CASE

1. **🎯 쿼리 목적**:
    
    - 키에 따라 키 등급을 분류
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, height,
           CASE 
               WHEN height >= 180 THEN '키 큼'
               WHEN height >= 170 THEN '보통'
               WHEN height IS NOT NULL THEN '키 작음'
               ELSE '미기재'
           END AS height_grade
    FROM userTbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `WHEN 조건 THEN 결과` 형식
        
    - 다양한 조건을 유연하게 처리 가능
        

---

#### 🔸 복합 조건 CASE

1. **🎯 쿼리 목적**:
    
    - 구매 금액에 따라 구매 유형 분류
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT prodName, price, amount,
           CASE
               WHEN price * amount >= 1000 THEN '고가 구매'
               WHEN price * amount >= 500 THEN '중간 구매'
               ELSE '일반 구매'
           END AS purchase_type
    FROM buyTbl;
    ```
    

---

## 🔢 집계 함수 (다중행 함수)

### 📌 기본 집계 사용

1. **🎯 쿼리 목적**:
    
    - 사용자 데이터의 전체 통계 분석
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT COUNT(*) AS user_count,
           COUNT(mobile1) AS mobile_count,
           MAX(height) AS max_height,
           MIN(height) AS min_height,
           AVG(height) AS avg_height,
           ROUND(STDDEV(height), 2) AS height_stddev,
           ROUND(VARIANCE(height), 2) AS height_var
    FROM userTbl;
    ```
    

---

### 📌 지역별 사용자 통계

1. **🎯 쿼리 목적**:
    
    - 지역별 사용자 수 및 키 통계 분석
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr,
           COUNT(*) AS user_count,
           MAX(height) AS max_height,
           MIN(height) AS min_height,
           ROUND(AVG(height), 1) AS avg_height
    FROM userTbl
    GROUP BY addr
    ORDER BY user_count DESC;
    ```
    

---

### 📌 출생연도별 사용자 통계

1. **🎯 쿼리 목적**:
    
    - 출생연도 기준 사용자 수 및 평균 키 분석
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT birthYear,
           COUNT(*) AS user_count,
           ROUND(AVG(height), 1) AS avg_height
    FROM userTbl
    GROUP BY birthYear
    ORDER BY birthYear;
    ```
    

---

### 📌 상품 그룹별 구매 통계

1. **🎯 쿼리 목적**:
    
    - 그룹명 기준 구매 정보 집계
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT groupName,
           COUNT(*) AS purchase_count,
           SUM(amount) AS total_amount,
           SUM(price * amount) AS total_price,
           ROUND(AVG(price), 0) AS avg_price
    FROM buyTbl
    WHERE groupName IS NOT NULL
    GROUP BY groupName
    ORDER BY total_price DESC;
    ```
    

---

### 📌 HAVING 절 사용

1. **🎯 쿼리 목적**:
    
    - 사용자별 총 구매금액이 100 이상인 경우만 추출
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userID, SUM(price * amount) AS total_purchase
    FROM buyTbl
    GROUP BY userID
    HAVING SUM(price * amount) >= 100
    ORDER BY total_purchase DESC;
    ```
    

---

## 📈 분석 함수 (Window Function)

### 📌 순위 관련 함수

1. **🎯 쿼리 목적**:
    
    - 키 기준 사용자 순위 부여
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, height,
           RANK() OVER (ORDER BY height DESC) AS height_rank,
           DENSE_RANK() OVER (ORDER BY height DESC) AS dense_rank,
           ROW_NUMBER() OVER (ORDER BY height DESC) AS row_num
    FROM userTbl;
    ```
    

---

### 📌 지역별 순위

1. **🎯 쿼리 목적**:
    
    - 지역 내 키 순위 분석
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, addr, height,
           RANK() OVER (PARTITION BY addr ORDER BY height DESC) AS local_rank
    FROM userTbl;
    ```
    

---

### 📌 이전/다음 행 참조

1. **🎯 쿼리 목적**:
    
    - 출생연도 순으로 앞뒤 사용자 확인
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, birthYear, height,
           LAG(name, 1, '없음') OVER (ORDER BY birthYear) AS prev_person,
           LEAD(name, 1, '없음') OVER (ORDER BY birthYear) AS next_person
    FROM userTbl;
    ```
    

---

### 📌 지역별 최대/최소 키 사용자

1. **🎯 쿼리 목적**:
    
    - 지역별 키 최대/최소 사용자 추출
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, addr, height,
           FIRST_VALUE(name) OVER (PARTITION BY addr ORDER BY height DESC) AS tallest_in_region,
           LAST_VALUE(name) OVER (
               PARTITION BY addr 
               ORDER BY height DESC
               RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
           ) AS shortest_in_region
    FROM userTbl;
    ```
    

---

### 📌 누적 구매 금액

1. **🎯 쿼리 목적**:
    
    - 사용자별 구매 누적 금액 계산
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userID, num, prodName, price * amount AS purchase_amount,
           SUM(price * amount) OVER (PARTITION BY userID ORDER BY num) AS cumulative_amount
    FROM buyTbl
    ORDER BY userID, num;
    ```
    

---

## ⚠️ 함수 사용 시 주의사항

### 📌 NULL 처리 주의

```sql
SELECT 
    COUNT(*) AS total_users,
    COUNT(mobile1) AS with_mobile,
    AVG(height) AS wrong_avg,
    SUM(height)/COUNT(*) AS real_avg
FROM userTbl;
```

- `AVG`는 NULL을 자동 제외함
    
- 전체 기준 평균을 원할 경우 `SUM / COUNT(*)` 사용
    

---

### 📌 암시적 형변환 주의

```sql
SELECT * FROM userTbl
WHERE birthYear = '1979';
```

- 문자형 비교로 인덱스 사용 불가 가능성 있음
    

---

### 📌 인덱스 사용 방해 함수

```sql
SELECT * FROM userTbl
WHERE SUBSTR(name, 1, 1) = '김';  -- 인덱스 사용 불가
```

- 아래와 같이 LIKE로 개선
    

```sql
SELECT * FROM userTbl
WHERE name LIKE '김%';
```

---

### 📌 단일행/다중행 함수 혼용 오류

```sql
SELECT name, AVG(height) FROM userTbl;  -- 오류
```

- 그룹화 필요
    

```sql
SELECT addr, AVG(height) FROM userTbl GROUP BY addr;
```

---

## 🧬 데이터베이스별 함수 차이

|목적|Oracle|SQL Server|MySQL|표준 SQL|
|---|---|---|---|---|
|문자열 연결|`||`|`+`|
|날짜 함수|`SYSDATE`|`GETDATE()`|`NOW()`|-|
|NULL 처리|`NVL()`|`ISNULL()`|`IFNULL()`|`COALESCE()`|

---

## 🧪 실습 예제 및 SQLD 기출 예시

### 📌 예제 1: 전화번호 형식화

```sql
SELECT name, 
    CASE 
        WHEN mobile1 IS NOT NULL THEN '(' || mobile1 || ') ' || SUBSTR(mobile2, 1, 4) || '-' || SUBSTR(mobile2, 5)
        ELSE '연락처 없음'
    END AS formatted_mobile
FROM userTbl;
```

---

### 📌 예제 2: 연령대별 사용자 수 및 평균 키

```sql
SELECT 
    FLOOR((2023 - birthYear) / 10) * 10 || '대' AS age_group,
    COUNT(*) AS user_count,
    ROUND(AVG(height), 1) AS avg_height
FROM userTbl
GROUP BY FLOOR((2023 - birthYear) / 10) * 10
ORDER BY age_group;
```

---

### 📌 예제 3: 가입년도별 사용자 수

```sql
SELECT 
    EXTRACT(YEAR FROM mDate) AS join_year,
    COUNT(*) AS user_count,
    ROUND(AVG(EXTRACT(YEAR FROM mDate) - birthYear), 1) AS avg_age_at_join
FROM userTbl
GROUP BY EXTRACT(YEAR FROM mDate)
ORDER BY join_year;
```

---

### 📌 예제 4: 사용자별 구매 금액 및 등급

```sql
SELECT u.userID, u.name,
       NVL(SUM(b.price * b.amount), 0) AS total_purchase,
       CASE 
           WHEN SUM(b.price * b.amount) > 1000 THEN 'VIP'
           WHEN SUM(b.price * b.amount) > 100 THEN '우수 고객'
           WHEN SUM(b.price * b.amount) > 0 THEN '일반 고객'
           ELSE '미구매 고객'
       END AS customer_grade
FROM userTbl u
LEFT JOIN buyTbl b ON u.userID = b.userID
GROUP BY u.userID, u.name
ORDER BY total_purchase DESC;
```

---

### 📌 예제 5: 상품 카테고리별 판매 현황

```sql
SELECT 
    NVL(groupName, '기타') AS category,
    COUNT(DISTINCT userID) AS customer_count,
    COUNT(*) AS purchase_count,
    SUM(amount) AS total_amount,
    SUM(price * amount) AS total_sales,
    ROUND(AVG(price), 2) AS avg_price,
    MAX(price) AS max_price
FROM buyTbl
GROUP BY ROLLUP(groupName);
```

---

### 📌 예제 6: 지역별 키가 가장 큰 사용자 찾기

```sql
SELECT addr, name, height
FROM (
    SELECT addr, name, height,
           RANK() OVER (PARTITION BY addr ORDER BY height DESC) AS height_rank
    FROM userTbl
) ranked
WHERE height_rank = 1;
```

---

### 📌 SQLD 기출 유형 - 문자열 처리 및 나이 계산

```sql
SELECT 
    userID,
    RPAD(SUBSTR(name, 1, 1), LENGTH(name), '*') AS masked_name,
    EXTRACT(YEAR FROM mDate) AS join_year,
    2023 - birthYear AS korean_age
FROM userTbl;
```

---

### 📌 SQLD 기출 유형 - 조건별 집계

```sql
SELECT addr,
       COUNT(*) AS user_count,
       COUNT(CASE WHEN height >= 175 THEN 1 END) AS tall_count,
       ROUND(AVG(height), 1) AS avg_height,
       ROUND(AVG(2023 - birthYear), 1) AS avg_age
FROM userTbl
GROUP BY addr
HAVING COUNT(*) >= 2
ORDER BY user_count DESC;
```