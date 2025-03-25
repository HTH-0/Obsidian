
---

## 🔎 그룹별 집계 쿼리

### 📌 주소별 사용자 수 조회

1. **🎯 쿼리 목적**:
    
    - 지역별 사용자 수를 집계
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT addr, COUNT(*) AS 사용자수
    FROM usertbl
    GROUP BY addr;
    ```
    
3. **🧠 세부 설명**:
    
    - `SELECT addr, COUNT(*)`: 주소별로 사용자 수 계산
        
    - `FROM usertbl`: 사용자 정보 테이블
        
    - `GROUP BY addr`: 주소(addr) 기준으로 그룹화
        

---

### 📌 그룹 이름별 총 판매액 계산

1. **🎯 쿼리 목적**:
    
    - 그룹명 기준으로 총 판매액 집계
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT groupName, SUM(price * amount) AS 판매액
    FROM buytbl
    WHERE groupName IS NOT NULL
    GROUP BY groupName;
    ```
    
3. **🧠 세부 설명**:
    
    - `SUM(price * amount)`: 가격 × 수량으로 총액 계산
        
    - `WHERE groupName IS NOT NULL`: 그룹명이 있는 항목만 집계
        
    - `GROUP BY groupName`: 그룹명 기준으로 그룹화
        
4. **💡 추가 팁**:
    
    - NULL 값이 포함되면 그룹이 따로 생기므로 필요 시 제외 처리
        

---

### 📌 출생 연도별 사용자 수 조회

1. **🎯 쿼리 목적**:
    
    - 태어난 연도 기준으로 인원 수 집계
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT birthYear, COUNT(*) "인원수"
    FROM usertbl
    GROUP BY birthYear
    ORDER BY birthYear;
    ```
    
3. **🧠 세부 설명**:
    
    - `COUNT(*)`: 연도별 사용자 수 계산
        
    - `ORDER BY birthYear`: 연도 오름차순 정렬
        
    - `"인원수"`: 컬럼 별칭을 큰따옴표로 지정해 한글/공백 허용
        

---

## 📋 정렬 관련 쿼리

### 📌 가입일 기준 정렬 (오름차순 / 내림차순)

1. **🎯 쿼리 목적**:
    
    - 사용자 가입일 순으로 정렬해서 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, mdate
    FROM usertbl
    ORDER BY mdate;
    ```
    
    ```sql
    SELECT name, mdate
    FROM usertbl
    ORDER BY mdate DESC;
    ```
    
3. **🧠 세부 설명**:
    
    - `ORDER BY mdate`: 가입일 기준 오름차순 정렬
        
    - `DESC`: 내림차순 정렬
        

---

### 📌 키(height) 기준 정렬 + 이름 기준 보조 정렬

1. **🎯 쿼리 목적**:
    
    - 키가 큰 순서대로 정렬하되, 같은 키일 경우 이름순 정렬
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT name, height
    FROM usertbl
    ORDER BY height DESC, name ASC;
    ```
    
3. **🧠 세부 설명**:
    
    - `ORDER BY height DESC`: 키 큰 사람 우선
        
    - `name ASC`: 이름을 오름차순으로 보조 정렬
        

---

## 🔢 ROWNUM 활용한 행 번호 필터링

### 📌 ROWNUM으로 특정 행만 추출

1. **🎯 쿼리 목적**:
    
    - 2번째 행의 데이터만 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM (SELECT rownum as RN, usertbl.* FROM usertbl) 
    WHERE RN = 2;
    ```
    
3. **🧠 세부 설명**:
    
    - `rownum`: Oracle에서 제공하는 가상 행 번호
        
    - 서브쿼리로 먼저 rownum을 부여한 뒤 WHERE 절에서 필터링
        

---

### 📌 전체 행에 ROWNUM 추가해서 조회

1. **🎯 쿼리 목적**:
    
    - 모든 행에 번호를 붙여 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT rownum AS RN, usertbl.* 
    FROM usertbl;
    ```
    
3. **🧠 세부 설명**:
    
    - `rownum AS RN`: 결과에 가상 행 번호 추가
        

---

### 📌 특정 범위의 행 번호 조회 (2~4행)

1. **🎯 쿼리 목적**:
    
    - 2~4번째 행 데이터 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT *
    FROM (SELECT rownum AS RN, usertbl.* FROM usertbl)
    WHERE RN >= 2 AND RN <= 4;
    ```
    
3. **🧠 세부 설명**:
    
    - 서브쿼리에서 먼저 rownum 부여
        
    - 외부 쿼리에서 번호 범위 조건 설정
        
4. **💡 추가 팁**:
    
    - ROWNUM은 정렬 후에 적용되지 않음
        
    - 정렬과 함께 사용할 경우 인라인 뷰로 먼저 정렬 처리 필요
        

---