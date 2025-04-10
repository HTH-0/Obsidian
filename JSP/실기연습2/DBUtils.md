# DBUtils.java 설명

## 📌 클래스 개요

- JDBC를 이용한 **오라클 DB 연동 유틸리티 클래스**
    
- DAO 역할을 하며, DB 연결과 SQL 실행을 담당
    
- **싱글톤 패턴(Singleton)** 사용 → 프로그램 전체에서 DB 연결 인스턴스를 하나만 유지
    

---

## 🔧 필드 구성

|필드명|역할|
|---|---|
|`url`|JDBC 접속 URL (`localhost`, SID: xe)|
|`id`, `pw`|DB 접속 계정 정보 (`system` / `1234`)|
|`conn`|DB 연결 객체|
|`pstmt`|SQL 실행 객체 (PreparedStatement)|
|`rs`|결과 집합 객체 (ResultSet)|

---

## 🧱 생성자 & 싱글톤

```java
private DBUtils() throws Exception {
	Class.forName("oracle.jdbc.driver.OracleDriver");
	conn = DriverManager.getConnection(url, id, pw);
}
```

- 생성자에서 **드라이버 로딩**과 **DB 연결 수행**
    
- 외부에서 직접 생성 못하게 `private`으로 설정
    

```java
public static DBUtils getInstance() throws Exception {
	if(instance == null) instance = new DBUtils();
	return instance;
}
```

- **getInstance()**를 통해서만 객체 생성 가능
    

---

## 📂 주요 기능별 메서드 정리

### 📘 1. `selectAllTeacher()`

- 테이블: `TBL_TEACHER_202201`
    
- 모든 강사 정보를 조회하여 `TeacherDto` 리스트로 반환
    

|컬럼명|DTO 필드|
|---|---|
|TEACHER_CODE|teacher_code|
|TEACHER_NAME|teacher_name|
|CLASS_NAME|class_name|
|CLASS_PRICE|class_price|
|TEACHER_REGIST_DATE|teacher_regist_date|

---

### 📘 2. `selectAllMember()`

- 테이블: `TBL_MEMBER_202201`
    
- 모든 회원 정보 조회 → `MemberDto` 리스트로 반환
    

|컬럼명|DTO 필드|
|---|---|
|C_NO|c_no|
|C_NAME|c_name|
|PHONE|phone|
|ADDRESS|address|
|GRADE|grade|

---

### 📘 3. `selectAllClass()`

- 테이블: `TBL_CLASS_202201`
    
- 등록된 수업 리스트 조회 → `ClassDto` 리스트로 반환
    

|컬럼명|DTO 필드|
|---|---|
|REGIST_MONTH|regist_month|
|C_NO|c_no|
|CLASS_AREA|class_area|
|TUITION|tuition|
|TEACHER_CODE|teacher_code|

---

### 📘 4. `insertClass(ClassDto classDto)`

- 새로운 수강 신청 데이터 삽입
    
- SQL: `INSERT INTO TBL_CLASS_202201 VALUES (?, ?, ?, ?, ?)`
    
- 필드 순서대로 DTO에서 데이터 추출 후 바인딩
    
- 수동 커밋 사용: `conn.commit()`
    

---

### 📘 5. `selectAllJoin1()`

- 조인 테이블 결과 조회 (회원 + 수업 + 강사)
    
- 사용 SQL:
    

```sql
SELECT C.REGIST_MONTH, M.C_NO, M.C_NAME, T.CLASS_NAME, C.CLASS_AREA, C.TUITION, M.GRADE
FROM TBL_MEMBER_202201 M
JOIN TBL_CLASS_202201 C ON C.C_NO = M.C_NO
JOIN TBL_TEACHER_202201 T ON C.TEACHER_CODE = T.TEACHER_CODE
```

- 반환 객체: `Join1Dto`
    

---

### 📘 6. `selectAllJoin2()`

- 강사별 수업 매출 합계 통계 조회
    
- 사용 SQL:
    

```sql
SELECT T.TEACHER_CODE, T.CLASS_NAME, T.TEACHER_NAME, SUM(C.TUITION)
FROM TBL_CLASS_202201 C
JOIN TBL_TEACHER_202201 T ON C.TEACHER_CODE = T.TEACHER_CODE
GROUP BY T.TEACHER_CODE, T.CLASS_NAME, T.TEACHER_NAME
ORDER BY SUM(C.TUITION) DESC
```

- 반환 객체: `Join2Dto`
    

---

## ✅ 핵심 정리

- **DAO + DB 연결 객체를 합친 통합 클래스**
    
- 주요 테이블: `TBL_TEACHER_202201`, `TBL_MEMBER_202201`, `TBL_CLASS_202201`
    
- 다양한 **SELECT / INSERT** 기능을 갖춘 실전형 클래스
    
- DTO 클래스를 통해 결과 객체 반환 (JavaBeans 구조)
    
- 싱글톤 패턴 구조는 유지하되, 실제 프로젝트에서는 커넥션 풀 사용 권장
    

---
