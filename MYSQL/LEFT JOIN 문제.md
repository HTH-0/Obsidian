# 📘 SELF JOIN을 활용한 조건별 비교

---

## 📌 같은 지역(addr)을 가진 다른 사용자 조회

1. **🎯 쿼리 목적**:  
    동일한 주소(addr)를 가진 서로 다른 사용자 쌍 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
    LEFT JOIN usertbl u2
      ON u1.addr = u2.addr
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `FROM usertbl u1 LEFT JOIN usertbl u2`: 같은 테이블을 자기 자신과 조인 (SELF JOIN)
        
    - `ON u1.addr = u2.addr`: 주소가 같은 사용자끼리 연결
        
    - `WHERE u1.userid <> u2.userid`: 자기 자신 제외 (동일 사용자 제거)
        
4. **💡 추가 팁**:
    
    - 자기 자신을 제외하는 `u1.userid <> u2.userid` 조건은 SELF JOIN에서 필수적
        
    - `LEFT JOIN`을 사용했지만 사실상 `INNER JOIN`처럼 작동함 (조건 때문에)
        

---

## 📌 키가 더 큰 사람과의 비교

1. **🎯 쿼리 목적**:  
    자신보다 키가 큰 사람 목록 비교
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT u1.name 나, u2.name "더 큰 사람", u1.height 내키, u2.height "걔 키"
    FROM usertbl u1
    LEFT JOIN usertbl u2
      ON u1.height < u2.height;
    ```
    
3. **🧠 세부 설명**:
    
    - `ON u1.height < u2.height`: u1보다 키가 큰 u2만 조인됨
        
    - `LEFT JOIN`: u1 기준이므로 자신보다 키 큰 사람이 없는 경우도 포함됨 (NULL 나올 수 있음)
        
    - `SELECT`: 두 사람의 이름과 키를 비교하여 출력
        
4. **💡 추가 팁**:
    
    - 자신보다 키가 큰 사람이 없으면 `u2` 정보는 NULL로 표시됨
        
    - 실무에서는 `WHERE u2.userid IS NOT NULL` 등을 붙여 필터링할 수 있음
        

---

## 📌 같은 출생연도(birthyear)를 가진 다른 사용자 조회

1. **🎯 쿼리 목적**:  
    같은 출생연도를 가진 다른 사용자 조회
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
    LEFT JOIN usertbl u2
      ON u1.birthyear = u2.birthyear
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `ON u1.birthyear = u2.birthyear`: 동일한 출생연도 조건
        
    - `WHERE u1.userid <> u2.userid`: 동일인 제거
        
4. **💡 추가 팁**:
    
    - 특정 연령대 사용자 그룹핑할 때 활용 가능
        

---

## 📌 같은 가입일(mdate)을 가진 다른 사용자 조회 (결과 없음)

1. **🎯 쿼리 목적**:  
    같은 가입일을 가진 다른 사용자 찾기 (하지만 결과 없음)
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
    LEFT JOIN usertbl u2
      ON u1.mdate = u2.mdate
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - 조인 조건은 가입일(mdate)이 같음
        
    - 결과가 없는 이유는 실제 데이터에 같은 가입일을 가진 사람이 없음
        
4. **💡 추가 팁**:
    
    - 결과가 없는 경우, 해당 컬럼의 중복 여부를 먼저 확인 필요
        

---

## 📌 같은 키(height)를 가진 다른 사용자 조회

1. **🎯 쿼리 목적**:  
    같은 키를 가진 사용자끼리 비교
    
2. **💻 SQL 구문**:
    
    ```sql
    SELECT * 
    FROM usertbl u1
    LEFT JOIN usertbl u2
      ON u1.height = u2.height
    WHERE u1.userid <> u2.userid;
    ```
    
3. **🧠 세부 설명**:
    
    - `ON u1.height = u2.height`: 동일 키 조건
        
    - `WHERE u1.userid <> u2.userid`: 자신 제외
        
4. **💡 추가 팁**:
    
    - 동일 키 사용자 그룹 찾기나 페어 구성 시 유용
        

---

> 참고: `LEFT JOIN`을 사용했지만 대부분의 쿼리는 `INNER JOIN`처럼 작동함 (WHERE절에서 NULL 필터링됨)  
> 조인 조건 기준 컬럼의 중복 존재 여부를 항상 확인할 것.