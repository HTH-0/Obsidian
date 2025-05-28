# 📘 GROUP BY 활용 정리

---

## 🔎 그룹별 데이터 집계

---

### 📌 주소별 사용자 수 조회

1. **🎯 쿼리 목적**:  
    지역(addr)별로 몇 명의 사용자가 있는지 집계
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, COUNT(userid)
    FROM usertbl
    GROUP BY addr;
    ```
    
3. **🧠 세부 설명**:
    
    - `SELECT addr, COUNT(userid)`: 각 주소별로 사용자 수 계산
        
    - `FROM usertbl`: 사용자 정보 테이블
        
    - `GROUP BY addr`: 주소별로 그룹화하여 집계 수행
        

---

### 📌 사용자별 총 구매금액 조회

1. **🎯 쿼리 목적**:  
    각 사용자(userid)의 전체 구매 금액 계산
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, SUM(price * amount)
    FROM buytbl
    GROUP BY userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `SUM(price * amount)`: 단가 × 수량으로 구매 금액 계산
        
    - `GROUP BY userid`: 사용자별로 그룹화하여 총액 집계
        

---

### 📌 그룹명별 구매 수 조회 (NULL 처리 포함)

1. **🎯 쿼리 목적**:  
    groupname 별 구매 건수 집계, null은 '미분류' 처리
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT NVL(groupname, '미분류') AS groupname, COUNT(amount)
    FROM buytbl
    GROUP BY groupname;
    ```
    
3. **🧠 세부 설명**:
    
    - `NVL(groupname, '미분류')`: null 그룹명을 '미분류'로 표시
        
    - `COUNT(amount)`: 구매 횟수 집계
        
    - `GROUP BY groupname`: 그룹별로 묶어서 집계
        

---

### 📌 출생연도별 평균 키 계산

1. **🎯 쿼리 목적**:  
    사용자들의 출생연도별 평균 키 계산
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT birthYear, AVG(height)
    FROM usertbl
    GROUP BY birthyear;
    ```
    
3. **🧠 세부 설명**:
    
    - `AVG(height)`: 평균 키
        
    - `GROUP BY birthyear`: 출생연도 기준으로 그룹화
        

---

### 📌 상품별 구매 횟수 및 총 구매금액

1. **🎯 쿼리 목적**:  
    상품별로 구매된 횟수와 총 구매금액 집계
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT prodname,
           COUNT(*) "구매 횟수",
           SUM(price * amount) "총 구매액"
    FROM buytbl
    GROUP BY prodname
    ORDER BY "구매 횟수" DESC;
    ```
    
3. **🧠 세부 설명**:
    
    - `COUNT(*)`: 구매된 횟수
        
    - `SUM(price * amount)`: 총 구매금액
        
    - `ORDER BY`: 구매 횟수 기준 내림차순 정렬
        

---

### 📌 휴대폰 번호 분류별 사용자 수

1. **🎯 쿼리 목적**:  
    휴대폰 앞자리(mobile1)별 사용자 수 집계, null은 '미분류'
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT NVL(mobile1, '미분류') AS mobile1, COUNT(*)
    FROM usertbl
    GROUP BY mobile1;
    ```
    
3. **🧠 세부 설명**:
    
    - `NVL(mobile1, '미분류')`: 휴대폰 정보 없는 경우 '미분류'로 처리
        
    - `COUNT(*)`: 사용자 수
        

---

### 📌 지역별 총 구매금액 조회 (JOIN 활용)

1. **🎯 쿼리 목적**:  
    사용자의 지역(addr)별로 전체 구매 금액 계산
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, SUM(price * amount)
    FROM buytbl B
    JOIN usertbl U ON U.userid = B.userid
    GROUP BY addr;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN`: 사용자 테이블과 구매 테이블 연결
        
    - `SUM(price * amount)`: 지역별 총 구매 금액 집계
        

---

### 📌 사용자별 구매한 상품 종류 수

1. **🎯 쿼리 목적**:  
    각 사용자가 구매한 **서로 다른 상품 종류** 개수 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT userid, COUNT(DISTINCT prodname)
    FROM buytbl
    GROUP BY userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `COUNT(DISTINCT prodname)`: 중복 없는 상품 수 계산
        

---

### 📌 가입일별 사용자 수

1. **🎯 쿼리 목적**:  
    가입일(mdate)별 사용자 수 확인
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT mdate, COUNT(userid)
    FROM usertbl
    GROUP BY mdate;
    ```
    
3. **🧠 세부 설명**:
    
    - `GROUP BY mdate`: 가입일 기준 그룹화
        

---

### 📌 출생연도별 총 구매 금액 (JOIN 활용)

1. **🎯 쿼리 목적**:  
    사용자의 출생연도별로 총 구매 금액 확인
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT birthyear, SUM(price * amount)
    FROM buytbl B
    JOIN usertbl U ON U.userid = B.userid
    GROUP BY birthyear;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN`: 사용자와 구매 정보 연결
        
    - `GROUP BY birthyear`: 출생연도 기준으로 집계
        

---