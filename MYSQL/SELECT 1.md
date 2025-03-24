---

# 📘 Oracle SQL Snippets 정리 모음

---

## 🔎 기본 조회 및 조건 조회

### 📌 전체 테이블 데이터 조회

1. **🎯 쿼리 목적**:  
    테이블의 모든 데이터를 확인
    
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl;
SELECT * FROM buytbl;
```

3. **🧠 세부 설명**:
    - `SELECT *`: 모든 컬럼을 조회
    - `FROM usertbl / buytbl`: 각각의 테이블에서 데이터 선택

---

### 📌 이름으로 특정 회원 조회

1. **🎯 쿼리 목적**:  
    이름이 '김경호'인 데이터 조회
    
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl WHERE NAME = '김경호';
```

3. **🧠 세부 설명**:
    - `WHERE NAME = '김경호'`: 이름이 '김경호'인 행만 필터링

---

### 📌 복수 조건으로 회원 필터링 (AND / OR)

1. **🎯 쿼리 목적**:  
    생년과 키를 조건으로 회원 조회
    
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl WHERE birthyear >= 1970 AND height >= 182;
SELECT * FROM usertbl WHERE birthyear >= 1970 OR height >= 182;
```

3. **🧠 세부 설명**:
    
    - `AND`: 두 조건을 모두 만족해야 조회됨
    - `OR`: 하나라도 만족하면 조회됨
4. **💡 추가 팁**:
    
    - 조건이 많아질 경우 괄호 `()`로 우선순위 설정 가능

---

### 📌 BETWEEN으로 범위 조회

1. **🎯 쿼리 목적**:  
    특정 범위 내 출생년도 필터링
    
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl WHERE birthyear BETWEEN 1970 AND 1980;
```

3. **🧠 세부 설명**:
    - `BETWEEN A AND B`: A 이상 B 이하 값 필터링
    - `birthyear BETWEEN 1970 AND 1980`: 1970년부터 1980년까지 출생자 조회

---

### 📌 IN 절로 여러 값 필터링

1. **🎯 쿼리 목적**:  
    특정 지역이나 휴대폰 앞자리에 해당하는 회원 필터링
    
2. **💻 SQL 구문**:
    

```sql
SELECT * FROM usertbl WHERE addr IN('경남', '전남', '경북');
SELECT * FROM usertbl WHERE mobile1 IN('010', '011');
```

3. **🧠 세부 설명**:
    - `IN (...)`: 여러 값 중 하나라도 일치하면 참
    - `addr IN(...)`: 주소가 목록에 포함되는 경우
    - `mobile1 IN(...)`: 휴대폰 번호 앞자리가 일치하는 경우

---

### 📌 이름 패턴으로 필터링 (LIKE)

1. **🎯 쿼리 목적**:  
    이름 패턴에 맞는 회원 필터링
    
2. **💻 SQL 구문**:
    

```sql
SELECT name, height FROM usertbl WHERE name LIKE '김%';
SELECT name, height FROM usertbl WHERE name LIKE '_재범';
```

3. **🧠 세부 설명**:
    
    - `LIKE '김%'`: '김'으로 시작하는 이름 (ex. 김민수, 김철수)
    - `LIKE '_재범'`: 앞 글자가 임의이고 '재범'으로 끝나는 이름 (ex. 이재범, 김재범)
4. **💡 추가 팁**:
    
    - `%`: 0개 이상의 임의 문자
    - `_`: 정확히 1개의 임의 문자
    - 와일드카드와 함께 `ESCAPE` 절을 쓰면 특수 문자도 검색 가능

---
