
# ✅ SQL 집합 연산자 완벽 정리

---

## 🔷 1. `UNION`

> 두 SELECT 결과를 합침 (중복 제거)

```sql
SELECT 컬럼 FROM 테이블1
UNION
SELECT 컬럼 FROM 테이블2;
```

- 결과: 두 집합의 **합집합 (중복 제거)**
    
- 정렬은 마지막에만 `ORDER BY`
    

---

## 🔷 2. `UNION ALL`

> 두 SELECT 결과를 합침 (중복 **포함**)

```sql
SELECT 컬럼 FROM 테이블1
UNION ALL
SELECT 컬럼 FROM 테이블2;
```

- 결과: 두 집합의 **단순 연결** (중복 OK)
    
- 속도 빠름 (중복 검사 안 함)
    

---

## 🔷 3. `INTERSECT`

> 두 SELECT 결과의 **공통된 행(교집합)**

```sql
SELECT 컬럼 FROM 테이블1
INTERSECT
SELECT 컬럼 FROM 테이블2;
```

- 결과: **두 SELECT에 모두 있는 행만 반환**
    
- 중복 제거됨 (교집합)
    

---

## 🔷 4. `MINUS` (Oracle) / `EXCEPT` (MySQL, PostgreSQL 등)

> **왼쪽 SELECT 결과에서 오른쪽 SELECT 결과를 뺌**

```sql
-- Oracle
SELECT 컬럼 FROM 테이블1
MINUS
SELECT 컬럼 FROM 테이블2;

-- PostgreSQL / SQL Server
SELECT 컬럼 FROM 테이블1
EXCEPT
SELECT 컬럼 FROM 테이블2;
```

- 결과: **A에는 있지만 B에는 없는 행만**
    
- 중복 제거됨 (차집합)
    

---

## ✅ 표로 정리

|연산자|의미|중복 제거|결과|
|---|---|---|---|
|`UNION`|합집합|✅|A + B (중복 X)|
|`UNION ALL`|합집합|❌|A + B (중복 O)|
|`INTERSECT`|교집합|✅|A ∩ B|
|`MINUS`/`EXCEPT`|차집합|✅|A - B|

---

## ✅ 예시 (같은 구조의 테이블 가정)

```sql
-- 예시 테이블
T1:             T2:

ID              ID
--              --
1               2
2               3
3               4
```

### `UNION`

```sql
SELECT ID FROM T1
UNION
SELECT ID FROM T2;
```

→ 결과: `1, 2, 3, 4` (중복 제거)

---

### `UNION ALL`

```sql
SELECT ID FROM T1
UNION ALL
SELECT ID FROM T2;
```

→ 결과: `1, 2, 3, 2, 3, 4` (중복 포함)

---

### `INTERSECT`

```sql
SELECT ID FROM T1
INTERSECT
SELECT ID FROM T2;
```

→ 결과: `2, 3`

---

### `MINUS` or `EXCEPT`

```sql
SELECT ID FROM T1
MINUS
SELECT ID FROM T2;
```

→ 결과: `1`

---

## ✅ 정리 문장

> `UNION`: 합쳐줘! (중복 제거)  
> `UNION ALL`: 그냥 다 붙여줘!  
> `INTERSECT`: 둘 다 가진 것만 줘  
> `MINUS`/`EXCEPT`: A에서 B를 빼줘!

---
