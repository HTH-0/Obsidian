# 📘 JOIN (SELF JOIN & CROSS JOIN 응용)

---

## 🔗 CROSS JOIN과 일반 JOIN 활용

### 📌 모든 행 조합 (CROSS JOIN)

1. **🎯 쿼리 목적**:  
    두 테이블 간 가능한 모든 조합을 생성
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT *
    FROM usertbl u
        CROSS JOIN buytbl b;
    ```
    
3. **🧠 세부 설명**:
    
    - `CROSS JOIN`: 조인 조건 없이 모든 행을 조합 (카티션 곱)
        
        - 예: usertbl에 3행, buytbl에 4행 → 결과는 12행
            
4. **💡 추가 팁**:
    
    - 실무에서는 테스트용 또는 제한 조건 있는 경우에만 사용
        
    - `ON`절 없음에 주의
        

---

### 📌 같은 테이블 간 JOIN (동일 userid)

1. **🎯 쿼리 목적**:  
    같은 사용자 ID를 가진 구매내역(buytbl)을 서로 비교
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT *
    FROM buytbl b
        JOIN buytbl bu
        ON b.userid = bu.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN buytbl bu ON b.userid = bu.userid`: 같은 사용자끼리의 행만 매칭
        
    - self-join을 사용해 같은 테이블을 비교
        

---

### 📌 같은 생년의 사용자 목록 (Self JOIN)

1. **🎯 쿼리 목적**:  
    출생연도가 같은 다른 사용자 쌍 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT *
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.birthyear = u2.birthyear
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN usertbl u2 ON u1.birthyear = u2.birthyear`: 같은 출생년도 사용자 매칭
        
    - `WHERE u1.userid <> u2.userid`: 자기 자신은 제외 (중복 제거 목적)
        
4. **💡 추가 팁**:
    
    - `<>`는 "같지 않음" 비교 연산자
        

---

### 📌 상하관계 매칭 (관리자 ID 활용 Self JOIN)

1. **🎯 쿼리 목적**:  
    usertbl 내에서 상사-부하 관계 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT *
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.managerid = u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `u1.managerid = u2.userid`: u1의 상사(u2)를 연결하는 self join
        

---

### 📌 상사명과 사원명 함께 출력

1. **🎯 쿼리 목적**:  
    상사-사원 관계를 이름 기준으로 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u.name 유저명, m.managerid 관리자명
    FROM userselftesttbl U
        JOIN userselftesttbl M
        ON u.userid = m.managerid;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN userselftesttbl M ON u.userid = m.managerid`: u가 m의 상사
        
    - `SELECT u.name, m.managerid`: 상사 이름, 부하의 관리자 ID 출력
        
4. **💡 추가 팁**:
    
    - 명확한 출력값을 위해 `JOIN` 뒤 alias 의미 파악 중요
        

---

### 📌 사원과 상사 이름 함께 출력 (조건 추가)

1. **🎯 쿼리 목적**:  
    상사(empid)와 부하직원(managerid)을 이름 기준으로 출력
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT e.empname 상사, m.empname 사원
    FROM emptbl e
        JOIN emptbl m
        ON e.empid = m.managerid
    WHERE e.managerid IS NOT NULL;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN emptbl m ON e.empid = m.managerid`: e가 m의 상사
        
    - `WHERE e.managerid IS NOT NULL`: 최고관리자 제외
        
4. **💡 추가 팁**:
    
    - 자기 자신이 최고 관리자면 `managerid IS NULL`로 관리자가 없음
        
    - 실무 조직도 출력 예제로 자주 등장
        

---