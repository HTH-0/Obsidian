`EXISTS`는 **서브쿼리의 결과가 존재하는지(boolean)**를 확인하는 조건절

---

## ✅ EXISTS 기본 문법

`SELECT 컬럼들 FROM 테이블A WHERE EXISTS (     SELECT 1     FROM 테이블B     WHERE 테이블A.기준컬럼 = 테이블B.기준컬럼       AND 조건 );`

- 서브쿼리에서 **결과가 1개 이상 나오면 TRUE**, 아니면 FALSE.
    
- `SELECT 1`은 그냥 관례일 뿐이고, 실제 리턴되는 값은 사용되지 않음 → 존재 여부만 확인함.