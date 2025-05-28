
---

## ✅ 주요 JOIN 종류별 차이 요약

|JOIN 종류|기준 테이블|조인 결과 특징|
|---|---|---|
|**INNER JOIN**|양쪽 다|**양쪽 모두 일치**하는 행만 반환|
|**LEFT OUTER JOIN**|왼쪽(A)|왼쪽은 무조건 나오고, 오른쪽이 없으면 `NULL`|
|**RIGHT OUTER JOIN**|오른쪽(B)|오른쪽은 무조건 나오고, 왼쪽이 없으면 `NULL`|
|**FULL OUTER JOIN** (DB에 따라 지원 X)|양쪽 다|양쪽 모두 포함 + 일치하지 않으면 `NULL` 채움|
|**CROSS JOIN**|없음|**모든 가능한 조합 (카티시안 곱)**|

---

## 🧪 실데이터 예제로 비교

### 📂 고객 테이블 A

|고객ID|이름|
|---|---|
|1|철수|
|2|영희|
|3|민수|

### 📂 주문 테이블 B

|주문ID|고객ID|상품명|
|---|---|---|
|101|1|사과|
|102|1|바나나|
|103|2|오렌지|

---

### 🔹 `INNER JOIN`

```sql
SELECT A.이름, B.상품명
FROM 고객 A
INNER JOIN 주문 B ON A.고객ID = B.고객ID;
```

✅ 결과: **양쪽에 모두 존재하는 고객만**

|이름|상품명|
|---|---|
|철수|사과|
|철수|바나나|
|영희|오렌지|

➡ 민수는 주문 정보가 없어서 빠짐

---

### 🔹 `LEFT OUTER JOIN`

```sql
SELECT A.이름, B.상품명
FROM 고객 A
LEFT OUTER JOIN 주문 B ON A.고객ID = B.고객ID;
```

✅ 결과: **고객은 모두 나옴, 주문 없으면 NULL**

|이름|상품명|
|---|---|
|철수|사과|
|철수|바나나|
|영희|오렌지|
|민수|NULL|

➡ 민수도 나오고, 주문이 없으면 `NULL`

---

### 🔹 `RIGHT OUTER JOIN`

```sql
SELECT A.이름, B.상품명
FROM 고객 A
RIGHT OUTER JOIN 주문 B ON A.고객ID = B.고객ID;
```

✅ 결과: **주문 기준 → 주문은 다 나오고, 고객 없으면 NULL**

|이름|상품명|
|---|---|
|철수|사과|
|철수|바나나|
|영희|오렌지|

➡ 지금은 고객이 다 있어서 INNER JOIN과 동일하지만,  
➡ 만약 주문 테이블에 고객ID가 999처럼 매칭 안 되는 값이 있으면, `이름 = NULL`로 나옴

---

### 🔹 `FULL OUTER JOIN` (PostgreSQL, Oracle 등에서만 지원)

```sql
SELECT A.이름, B.상품명
FROM 고객 A
FULL OUTER JOIN 주문 B ON A.고객ID = B.고객ID;
```

✅ 결과: **둘 다 포함 + 매칭 안 되면 NULL**

---

## 📌 핵심 차이 정리

|JOIN 종류|A만 있는 값 포함?|B만 있는 값 포함?|
|---|---|---|
|INNER JOIN|❌|❌|
|LEFT JOIN|✅|❌|
|RIGHT JOIN|❌|✅|
|FULL JOIN|✅|✅|

---

## ✅ 결론

- `INNER JOIN`은 **일치하는 것만**
    
- `LEFT OUTER JOIN`은 **왼쪽 기준 + 오른쪽 NULL 가능**
    
- `RIGHT OUTER JOIN`은 **오른쪽 기준 + 왼쪽 NULL 가능**
    
- `FULL OUTER JOIN`은 **양쪽 다 포함**
    

---
