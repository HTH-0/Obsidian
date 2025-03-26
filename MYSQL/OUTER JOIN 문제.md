# 📘 OUTER JOIN (Self Join과 조건 비교 활용)

---

## 🔁 자기 자신과 비교하는 JOIN

### 📌 같은 주소에 사는 다른 사용자 찾기

1. **🎯 쿼리 목적**:  
    동일한 주소(addr)에 거주하지만 서로 다른 사용자 쌍 찾기
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.addr = u2.addr
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `JOIN ON u1.addr = u2.addr`: 주소가 같은 사용자 매칭
        
    - `WHERE u1.userid <> u2.userid`: 자기 자신 제외
        

---

### 📌 나보다 키가 큰 사람 조회

1. **🎯 쿼리 목적**:  
    모든 사용자에 대해 자신보다 키가 큰 상대 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u1.name 나, u2.name "더 큰 사람", u1.height 내키, u2.height "걔 키"
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.height < u2.height;
    ```
    
3. **🧠 세부 설명**:
    
    - `u1.height < u2.height`: 키가 더 큰 사용자만 매칭
        
    - 모든 사용자 쌍에 대해 비교
        
4. **💡 추가 팁**:
    
    - `>` 비교로 바꾸면 반대로 "나보다 키 작은 사람"도 조회 가능
        

---

### 📌 같은 출생연도인 사용자 쌍 조회

1. **🎯 쿼리 목적**:  
    같은 출생연도지만 서로 다른 사용자 찾기
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.birthyear = u2.birthyear
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `ON u1.birthyear = u2.birthyear`: 같은 연도 매칭
        
    - `<>`: 자기 자신 제외
        

---

### 📌 가입일이 같은 사용자 쌍 찾기

1. **🎯 쿼리 목적**:  
    동일한 가입일(mdate)을 가진 서로 다른 사용자 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.mdate = u2.mdate
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - 가입일이 완전히 같은 경우가 없어서 결과가 없음
        
4. **💡 추가 팁**:
    
    - 이런 경우를 위해 `LEFT OUTER JOIN`으로 한쪽이라도 보여주는 방식 고려 가능
        

---

### 📌 키가 같은 사용자 쌍 조회

1. **🎯 쿼리 목적**:  
    동일한 키를 가진 다른 사용자 찾기
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
        JOIN usertbl u2
        ON u1.height = u2.height
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - 키 기준으로 self join
        
    - 자기 자신 제외 조건으로 중복 방지
        

---

## 💡 보충 팁: OUTER JOIN 언급 없이 INNER JOIN 구조인 이유

> 위 쿼리들은 `OUTER JOIN` 주제에 포함되었지만 실제 구문은 전부 `INNER JOIN` 형태  
> **OUTER JOIN을 사용해야 하는 경우**:

- 한쪽 테이블에만 데이터가 있는 경우도 포함하고 싶을 때
    
- 예: 구매 기록이 없는 사용자도 포함하고 싶을 때 `LEFT OUTER JOIN` 사용
    

---