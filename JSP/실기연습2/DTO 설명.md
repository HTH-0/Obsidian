
---

# ✅ 1. ClassDto.java

### 📦 역할

- 수업 정보를 저장하는 **DTO 클래스**
    
- `TBL_CLASS_202201` 테이블의 한 행(row)을 표현
    

### 📑 필드 설명

- `regist_month`: 수업 등록 월 (예: `"202201"`)
    
- `c_no`: 수강생 번호 (예: `"C001"`)
    
- `class_area`: 수업 지역 (예: `"강남"`)
    
- `tuition`: 수강료 (예: `"300000"`)
    
- `teacher_code`: 강사 코드 (예: `"T01"`)
    

### 🔧 기능

- 기본 생성자 + 모든 필드 초기화 생성자 제공
    
- 모든 필드에 대해 `getter`, `setter` 제공
    
- 객체 정보 확인을 위한 `toString()` 오버라이딩
    

---

# ✅ 2. DBUtils.java

### 📦 역할

- 데이터베이스에 접근하는 **DAO 유틸리티 클래스**
    
- 오라클 DB 연결, 쿼리 실행, DTO로 변환까지 담당
    
- **싱글톤 패턴** 사용 → DB 연결 객체는 1개만 생성
    

### ⚙️ 주요 구성

- DB 연결 정보 저장: `url`, `id`, `pw`
    
- JDBC 객체: `Connection`, `PreparedStatement`, `ResultSet`
    
- 정적 메서드 `getInstance()` 통해 싱글톤 인스턴스 반환
    

### 📌 주요 메서드

|메서드명|설명|
|---|---|
|`selectAllTeacher()`|강사 전체 조회 → `TeacherDto` 리스트로 반환|
|`selectAllMember()`|수강생 전체 조회 → `MemberDto` 리스트로 반환|
|`selectAllClass()`|수업 전체 조회 → `ClassDto` 리스트로 반환|
|`insertClass(ClassDto)`|수업 신규 등록 (INSERT 실행)|
|`selectAllJoin1()`|회원 + 수업 + 강사 테이블 조인 (등록정보 포함)|
|`selectAllJoin2()`|강사별 총 수강료 합계 (GROUP BY + ORDER BY)|

### 🧠 특이사항

- DB 연결 시 `Class.forName("oracle.jdbc.driver.OracleDriver")` 사용
    
- SQL 쿼리는 코드 내 문자열로 작성 (하드코딩)
    
- `commit()` 직접 수행 → 자동 커밋 아님
    

---

# ✅ 3. Join1Dto.java

### 📦 역할

- 회원 + 수업 + 강사 정보를 조인한 결과를 담는 DTO
    
- `selectAllJoin1()`의 결과를 표현하기 위해 사용
    

### 📑 필드 설명

- `regist_month`: 수업 등록 월
    
- `c_no`: 회원 번호
    
- `c_name`: 회원 이름
    
- `class_name`: 수업 이름
    
- `class_area`: 수업 지역
    
- `tuition`: 수강료
    
- `grade`: 회원 등급
    

### 🧠 특이사항

- 다양한 테이블 조인 결과를 하나의 객체로 구성함
    
- 단일 테이블이 아닌 조인 결과 전용 DTO
    

---

# ✅ 4. Join2Dto.java

### 📦 역할

- 강사별 수업 총 수강료 정보를 담는 DTO
    
- `selectAllJoin2()`의 결과를 표현
    

### 📑 필드 설명

- `teacher_code`: 강사 코드
    
- `class_name`: 수업 이름
    
- `teacher_name`: 강사 이름
    
- `total_tuition`: 총 수강료 (SUM)
    

### 🧠 특이사항

- `GROUP BY`와 `SUM` 결과를 표현하기 위해 만들어진 DTO
    
- 정렬 결과까지 표현 가능 (ORDER BY 절로 정렬된 리스트)
    

---

# ✅ 5. MemberDto.java

### 📦 역할

- 수강생 정보를 저장하는 DTO 클래스
    
- `TBL_MEMBER_202201` 테이블의 한 행을 표현
    

### 📑 필드 설명

- `c_no`: 회원 번호
    
- `c_name`: 회원 이름
    
- `phone`: 연락처
    
- `address`: 주소
    
- `grade`: 등급 (예: `"VIP"`, `"A"` 등)
    

### 🔧 기능

- 기본 생성자 + 전체 초기화 생성자
    
- 모든 필드에 대한 getter/setter
    
- `toString()` 오버라이딩
    

---

# ✅ 6. TeacherDto.java

### 📦 역할

- 강사 정보를 담는 DTO
    
- `TBL_TEACHER_202201` 테이블의 한 행을 표현
    

### 📑 필드 설명

- `teacher_code`: 강사 코드
    
- `teacher_name`: 강사 이름
    
- `class_name`: 강의명
    
- `class_price`: 강의 단가 (정수형)
    
- `teacher_regist_date`: 강사 등록일
    

### 🔧 기능

- 기본 생성자 + 전체 필드 초기화 생성자
    
- getter/setter + `toString()` 구현
    

---

# ✅ 7. VoteDto.java

### 📦 역할

- 투표자 정보를 담는 DTO 클래스
    
- (예: 주민번호 기반 투표 정보 관리)
    

### 📑 필드 설명

- `v_jumin`: 주민번호
    
- `v_name`: 성명
    
- `m_no`: 후보번호
    
- `v_time`: 투표시간
    
- `v_area`: 투표장소
    
- `v_confirm`: 유권자 확인 여부 (`"Y"`/`"N"`)
    

### 🔧 기능

- 기본 생성자 + 전체 필드 초기화 생성자
    
- getter/setter + `toString()` 구현
    

---
