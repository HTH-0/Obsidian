
---

### 📌 📊 그룹별 + 전체합계(ROLLUP) 집계 (NVL 사용)

1. **🎯 쿼리 목적**:
    
    - 상품 그룹별 총 구매금액과 전체 총합을 함께 조회
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT NVL(groupname, '총합') AS 그룹명,
           SUM(price * amount)
    FROM buytbl
    GROUP BY ROLLUP(groupname);
    ```
    
3. **🧠 세부 설명**:
    
    - `ROLLUP(groupname)`: groupname 기준으로 그룹핑 + 마지막에 전체합(총합) 추가
        
    - `NVL(groupname, '총합')`: groupname이 NULL인(즉, 총합행) 경우 '총합'으로 표시
        
    - `SUM(price * amount)`: 각 그룹의 총 구매금액 집계
        
4. **💡 추가 팁**:
    
    - `ROLLUP`은 다단계 집계를 위해 자주 사용
        
    - `NVL`은 NULL 값을 대체할 때 사용
        

---

### 📌 🧠 GROUPING 함수로 NULL 구분 (CASE + ROLLUP)

1. **🎯 쿼리 목적**:
    
    - 그룹명이 NULL인 경우 '미분류', ROLLUP 총합인 경우 '총합'으로 구분하여 출력
        
2. **💻 SQL 구문**:
    
    ```sql
    SELECT 
        CASE 
            WHEN GROUPING(groupname) = 1 THEN '총합'
            WHEN groupname IS NULL THEN '미분류'
            ELSE groupname
        END AS 그룹명,
        SUM(price * amount) AS 총금액
    FROM buytbl
    GROUP BY ROLLUP(groupname);
    ```
    
3. **🧠 세부 설명**:
    
    - `GROUPING(groupname)`: ROLLUP으로 생성된 총합 행인 경우 1 반환
        
    - `CASE WHEN`:
        
        - 총합인 경우 `'총합'`
            
        - NULL이지만 ROLLUP이 아닌 경우 `'미분류'`
            
        - 그 외는 원래 그룹명
            
    - `GROUP BY ROLLUP(groupname)`: 그룹별 + 전체합 계산
        
4. **💡 추가 팁**:
    
    - `GROUPING()` 함수는 ROLLUP/ROLLUP+GROUPING SETS에서 필수적으로 사용됨
        
    - 단순히 `NVL()`로는 총합과 미분류(NVL NULL) 구분이 어려울 수 있으므로 `GROUPING()`으로 구분 추천
        

---