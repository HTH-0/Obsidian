## ✅ CASE 표현식 정리

### 1. 단순(Simple) CASE 표현식

- **특정 컬럼의 값**에 따라 분기할 때 사용
    
- 형식:
    
    ```sql
    CASE 컬럼명
        WHEN 값1 THEN 결과1
        WHEN 값2 THEN 결과2
        ...
        ELSE 결과N
    END
    ```
    
- 예시:
    
    ```sql
    CASE job
        WHEN 'MANAGER' THEN '관리자'
        WHEN 'CLERK' THEN '사무원'
        ELSE '기타'
    END
    ```
    

---

### 2. 검색(Searched) CASE 표현식

- **복잡한 조건식**이 필요할 때 사용
    
- 형식:
    
    ```sql
    CASE
        WHEN 조건1 THEN 결과1
        WHEN 조건2 THEN 결과2
        ...
        ELSE 결과N
    END
    ```
    
- 예시:
    
    ```sql
    CASE
        WHEN salary > 5000 THEN '고소득'
        WHEN salary > 3000 THEN '중소득'
        ELSE '저소득'
    END
    ```
    

---

### 💡 변환 가능 여부

- 조건이 단순히 특정 컬럼 값과의 비교(`컬럼 = 값`)만 포함되어 있다면,  
    **검색 CASE → 단순 CASE**로 변환 가능.
    
- 복잡한 논리(`AND`, `OR`, 비교 연산 조합 등)는 **단순 CASE로 변환 불가**.